# db-cache
A PHP library to cache database query, supports MySQL/Mongo and Memcached/Redis...

##### [中文版](https://github.com/flashytime/db-cache/blob/master/README.zh-cn.md)

### Features
- Supports common databases, such as MySQL、Mongo
- Supports common cache servers, such as Memcached、Redis
- Supports database master-slave and reading/writing separation
- Supports MySQL database table sharding
- Manage cache through a `version` strategy, which controls the creation and expiration of the cache

### Installation
- Run the command below in your project root directory

```bash
composer require flashytime/db-cache
```
- Copy `config/db-cache.php` to your project config directory

### Usage

```php
class TestModel
{
    public $dbCache;

    public function __construct()
    {
        // you can get the config in your own way
        $config = $this->getConfig();
        $this->dbCache = new \Flashytime\DbCache\DbCache($config, 'Test', ['name' => 'test', 'primary' => 'id']);
    }

    public function create($data)
    {
        return $this->dbCache->insert($data);
    }

    public function getById($id)
    {
        return $this->dbCache->select(['where' => 'id = :id'], ['id' => $id]);
    }

    public function findAll($offset, $limit)
    {
        return $this->dbCache->all(['limit' => ':offset, :limit', 'order' => 'id DESC'], ['offset' => $offset, 'limit' => $limit]);
    }

    public function updateById($id, $data)
    {
        return $this->dbCache->update(['where' => 'id = :id'], ['id' => $id], $data);
    }

    public function remove($id)
    {
        return $this->dbCache->delete(['where' => 'id = :id'], ['id' => $id]);
    }

    public function getConfig()
    {
        //path to your config directory
        return require __DIR__ . '/../config/db-cache.php';
    }
}
```

### License
MIT