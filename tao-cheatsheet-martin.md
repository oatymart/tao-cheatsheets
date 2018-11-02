# TAO Snippets & Tips

## Installation of TAO

### Installing TAO via CLI

```shell
git clone https://oat-sa/package-tao.git myfolder
cd myfolder
git checkout develop
composer install
```

then

```shell
php tao/scripts/taoInstall.php \
--db_driver pdo_mysql \
--db_host localhost \
--db_name tao1 \
--db_user root \
--db_pass root \
--module_namespace http://sample/first.rdf \
--module_url http://localhost:8888/tao1 \
--user_login admin \
--user_pass admin \
-e taoCe,taoDevTools \
-v
```

In some cases you will first need to `mkdir data && mkdir config` to allow the install to run successfully.

### Installing TAO using config.json file

This assumes you use MAMP, details may vary.
I needed to edit `config.json`, specifically updating:

```json
    "configuration": {
        "global": {
            "instance_name": "tao3",
            "namespace": "http://tao.local/mytao.rdf",
            "url": "http://localhost:8888/tao3/",
            "file_path": "/path/to/my/htdocs/tao/data/",
            "root_path": "/path/to/my/htdocs/tao/"
        },
        "generis": {
            "persistences": {
                "default": {
                    "driver": "pdo_mysql",
                    "host": "localhost",
                    "port": "3306",
                    "dbname": "tao-db3",
                    "user": "martin",
                    "password": "mypassword"
                }
            }
        }
    }

```

### Installing in a Docker container

TODO

## Installing an extension

### From within TAO

(not verified yet)

In your TAO root folder:

`git clone https://github.com/oat-sa/extension-tao-test-extension.git taoTestExtension`

TAO BE > Settings > Extensions Manager. Install using the checkboxes.

### From the command line

(not verified yet)

```shell
git clone https://github.com/oat-sa/extension-tao-test-extension.git taoTestExtension
cd taoTestExtension
composer install
php tao/scripts/taoExtensions.php -v -u=admin -p=admin -a=install -e=taoTestExtension
```

(dependencies may not be satisfied automatically...)

### Updating

Best is to run `composer require repo/extension=branch`

Which must be followed by `php tao/scripts/taoUpdate.php` to let TAO know about it.

### Uninstalling (hackish)

Not recommended but sometimes useful if you need to reinstall a higher version you have downloaded.

- Delete the extension's entry in `/config/generis/installation.conf.php`
- Delete the extension's files `rm -rf taoMyExtension`

This method can create problems in some cases.

A safer trick is to lower the version of your extension listed in `/config/generis/installation.conf.php`. You will then be able to run its updater script by calling `php tao/scripts/taoUpdate.php`.

## Configuration of TAO

### Turning on TAO's logging

Open `/config/generis/log.conf.php`

and paste in code block from https://hub.taocloud.org/techdocs/generis/logger

e.g.

```php
return new oat\oatbox\log\LoggerService(array(
    'logger' => array(
        'class' => 'oat\\oatbox\\log\\logger\\TaoMonolog',
        'options' => array(
            'name' => 'tao',
            'handlers' => array(
                array(
                    'class' => 'Monolog\\Handler\\StreamHandler',
                    'options' => array(
                        '/var/www/tao/tao.log',
                        100
                    )
                ),
            )
        )
    )
));
```

View above log file:

`tail -f /private/var/www/tao/package-tao/test-log.log`

or install e.g. `lnav` CLI-tool for aggregating several logfiles

### Clearing cache

TAO's application cache can be cleared in TAO BackOffice by going to `Tools > Scripts > Empty cache`.

If something is causing TAO to crash or 500-Error, you can clear the cache manually by deleting the contents of `/generis/data/cache`.

### PDF in TAO

(Leave this until later)

Install PDFjs with a shell script:
https://hub.taocloud.org/articles/third-party-tools-and-libraries/install-pdfjs-viewer

### MathJax in TAO

(Leave this until later)

If you need MathJax, download and run the install script referenced here: https://hub.taotesting.com/articles/third-party-tools-and-libraries/enable-math-expression-in-items

It should work instantly.

If you just need to run a grunt or npm task without errors, you can just create an empty file:

`touch taoQtiItem/views/js/mathjax/MathJax.js`.

## Frontend Tooling

### TAO's ESlint config

The rules file is located at `/tao/views/build/.eslintrc.json`

Recommend copying it to your home folder so it is used globally in your editors.

### IDE style enforcement

TODO

### Grunt - compiling SASS, bundling JS, running QUnit tests

To get started:

```shell
cd tao/views/js/build
npm install
```

Now you have Grunt and all its dependencies installed in TAO.

#### SASS - compile some SCSS files

```shell
grunt sassall
grunt sass:extensionname
```

The output is generally stored in `views/css` of each extension you ran it for.

*Do not edit raw CSS! Always SCSS.*

#### QUnit - run Front End unit tests

`grunt connect:testkeepalive: -testPort=8085`

Then Ctrl-click the Terminal link to 127.0.0.1:8085 and browse the html tests.

or for CLI tests:

```shell
grunt testall
grunt test:extensionname
```

#### Grunt Watch

TODO

### Requirejs

TODO

## Modifying a TAO extension, pushing code

From your TAO root, get your extension set up:

```shell
git clone https://oat-sa/extension-tao-myextension.git myExtension
cd myExtension
composer update
git checkout -B fix/TAO-issueNumber-descriptive-name
```

Another guide to the naming convention of branches is on [this page](https://hub.taotesting.com/articles/faq/extension-update-guide).

Then edit code, `git commit` etc.

`git push origin fix/TAO-issueNumber-descriptive-name`

Make a pull request to the `develop` branch on GH and assign Assignees/Reviewers.

## Misc

### Delay media loading

Add the following section inside `tao/getFileFlysystem.php`:

```php
if (strpos($url, '.mp3') !== false) {
    sleep(7);
}
```

Mp3 files will then be delayed by 7 seconds.
