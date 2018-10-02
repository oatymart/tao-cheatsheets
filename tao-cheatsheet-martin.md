# TAO Snippets & Tips

## TAO's ESlint config
is located at `/tao/views/build/.eslintrc.json`
Recommend copying it to your home folder so it is used globally in your editors.


## IDE style enforcement
TODO


## Installing TAO via CLI
namespace needed to be set - not 100% sure what value it should really be...


## Installing TAO using config.json file
This assumes you use MAMP, details may vary.
I needed to edit `config.json`, specifically updating:
```
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


## Requirejs
TODO