# Библиотека для быстрого старта с RabbitMQ 

## Общая информация
Библиотека содержит базовый функционал для работы с менеджером очередей RabbitMQ. <br/>

## Изменения
| Версия   |      Дата     |  Изменения |
|----------|:-------------:|------:|
| dev-master |  2017-11-13 | Инициализация проекта |


## Зависимости
- PHP 5.6.0 и выше


## Установка
Используя Composer:<br/>
Созадйте или дополните уже существующий файл зависимостей следующей конструкцией

```
{
  "require": {
    "panace9i/rabbitmq": "dev-master"
  }
}
```
Выполните команду
```
composer install
```

## Пример

### Синхронный режим

#### Producer

Стандартное обращение
```
require "vendor/autoload.php";

use Panace9i\Queue\RabbitMQ\Producer\Adapter\Sync;

Panace9i\Queue\RabbitMQ\Config\Config::init()
  ->setHost('localhost')
  ->setPort(5672)
  ->setUser('guest')
  ->setPassword('guest');

$entity = new Sync();
$result = $entity->execute(<request_data_as_string>, <queue_name>);

...
```

С использованием фабрики
```
<?php
require "vendor/autoload.php";

use Panace9i\Queue\RabbitMQ\Producer\Factory AS ProducerFactory;

Panace9i\Queue\RabbitMQ\Config\Config::init()
  ->setHost('localhost')
  ->setPort(5672)
  ->setUser('guest')
  ->setPassword('guest');

$entity = ProducerFactory::getInstance(ProducerFactory::ADAPTER_SYNC);
$result = $entity->execute(<request_data_as_string>, <queue_name>);

...
```

#### Consumer

Стандартное обращение
```
<?php
require "vendor/autoload.php";

use Panace9i\Queue\RabbitMQ\Consumer\Adapter\Sync;
use Panace9i\Queue\RabbitMQ\Handler\HandlerAbstract;

Panace9i\Queue\RabbitMQ\Config\Config::init()
  ->setHost('localhost')
  ->setPort(5672)
  ->setUser('guest')
  ->setPassword('guest');

class SyncTest extends HandlerAbstract
{
    public function listen($request)
    {
        $params = $request->body;

        ...

        return $this->reply(<response_data_as_string>, $request);
    }
}

$entity = new Sync();
$entity->listen(<queue_name>, [new SyncTest, 'listen']);
```

С применением фабрики
```
require "vendor/autoload.php";

use Panace9i\Queue\RabbitMQ\Consumer\Factory AS ConsumerFactory;
use Panace9i\Queue\RabbitMQ\Handler\HandlerAbstract;

Panace9i\Queue\RabbitMQ\Config\Config::init()
  ->setHost('localhost')
  ->setPort(5672)
  ->setUser('guest')
  ->setPassword('guest');

class SyncTest extends HandlerAbstract
{
    public function listen($request)
    {
        $params = $request->body;

        ...

        return $this->reply(<response_data_as_string>, $request);
    }
}

$entity = ConsumerFactory::getInstance(ConsumerFactory::ADAPTER_SYNC);
$entity->listen(<queue_name>, [new SyncTest, 'listen']);
```


### Асинхронный режим

#### Producer

Стандартное обращение
```
require "vendor/autoload.php";

use Panace9i\Queue\RabbitMQ\Producer\Adapter\Async;

Panace9i\Queue\RabbitMQ\Config\Config::init()
  ->setHost('localhost')
  ->setPort(5672)
  ->setUser('guest')
  ->setPassword('guest');

$entity = new Async();
$result = $entity->execute(<request_data_as_string>, <queue_name>);

...
```

С применением фабрики
```
require "vendor/autoload.php";

use Panace9i\Queue\RabbitMQ\Producer\Factory AS ProducerFactory;

Panace9i\Queue\RabbitMQ\Config\Config::init()
  ->setHost('localhost')
  ->setPort(5672)
  ->setUser('guest')
  ->setPassword('guest');

$entity = ProducerFactory::getInstance(ProducerFactory::ADAPTER_ASYNC);
$entity->execute(<request_data_as_string>, <queue_name>);

...
```

#### Consumer

Стандартное обращение
```
require "vendor/autoload.php";

use Panace9i\Queue\RabbitMQ\Consumer\Adapter\Async;
use Panace9i\Queue\RabbitMQ\Handler\HandlerAbstract;

Panace9i\Queue\RabbitMQ\Config\Config::init()
  ->setHost('localhost')
  ->setPort(5672)
  ->setUser('guest')
  ->setPassword('guest');

class AsyncTest extends HandlerAbstract
{
    public function listen($request)
    {
        $params = $request->body;

        ...

        return $this->reply(<response_data_as_string>, $request);
    }
}

$entity = new Async();
$entity->listen(<queue_name>, [new AsyncTest, 'listen']);
```

С применением фабрики
```
require "vendor/autoload.php";

use Panace9i\Queue\RabbitMQ\Consumer\Factory AS ConsumerFactory;
use Panace9i\Queue\RabbitMQ\Handler\HandlerAbstract;

Panace9i\Queue\RabbitMQ\Config\Config::init()
  ->setHost('localhost')
  ->setPort(5672)
  ->setUser('guest')
  ->setPassword('guest');

class AsyncTest extends HandlerAbstract
{
    public function listen($request)
    {
        $params = $request->body;

        ...

        return $this->reply(<response_data_as_string>, $request);
    }
}

$entity = ConsumerFactory::getInstance(ConsumerFactory::ADAPTER_ASYNC);
$entity->listen(<queue_name>, [new AsyncTest, 'listen']);
```
