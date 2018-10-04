# TAO Snippets & Tips

## TAO's ESlint config

is located at `/tao/views/build/.eslintrc.json`
Recommend copying it to your home folder so it is used globally in your editors.

## IDE style enforcement

TODO

## Installing TAO via CLI

```shell
git clone ...

composer install
```

then

```shell
php tao/scripts/taoInstall.php \
--db_driver pdo_mysql \
--db_host localhost \
--db_name tao5 \
--db_user root \
--db_pass root \
--module_namespace http://sample/first.rdf \
--module_url http://localhost:8888/tao5 \
--user_login admin \
--user_pass admin \
-e taoCe,taoDevTools \
-v
```

## Installing TAO using config.json file

This assumes you use MAMP, details may vary.
I needed to edit `config.json`, specifically updating:

```json
    "configuration": {
        "global": {
            "instance_name": "tao3",
            "namespace": "http://tao.local/mytao.rdf",
            "url": "http://localhost:8888/tao3/",
            "file_path": "/path/to/my/htdocs/tao/data/",
            "root_path": "/path/to/my/htdocs/tao/",
        "generis": {
            "persistences": {
                "default": {
                    "driver": "pdo_mysql",
                    "host": "localhost",
                    "port": "3306",
                    "dbname": "tao-db3",
                    "user": "martin",
                    "password": "mypassword"
```

## Clearing cache

TAO's application cache can be cleared in TAO BackOffice by going to `Tools > Scripts > Empty cache`.

If something is causing TAO to crash or 500-Error, you can clear the cache manually by deleting the contents of `/generis/data/cache`.

## Turn on TAO's logging

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
                        '/var/www/tao/package-tao/test-log.log',
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
or install e.g. `lnav` CLI-tool for same purpose

## Requirejs

TODO

## QUnit

TODO

## PDF in TAO

Install PDFjs with a shell script:
https://hub.taocloud.org/articles/third-party-tools-and-libraries/install-pdfjs-viewer
