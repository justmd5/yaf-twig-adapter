# yaf-twig-adapter

Twig Adapter for Yaf PHP Framework with namespace enabled.


## Installation

You can install via Composer.

At first create `composer.json` file:

```json
{
	"require": {
		"lovelock/php-yaf-twig": ">=1.0"
	}
}
```

Run composer to install.

```
$ composer install
```

Finally, include `vendor/autoload.php` at `index.php`

```
require_once 'vendor/autoload.php';
```

Add to `Bootstrap.php`:

```php
<?php

use \Yaf\Bootstrap_Abstract;
use \Yaf\Dispatcher;
use \Yaf\Application;
use \JustMd5\TwigAdapter;

class Bootstrap extends Bootstrap_Abstract
{

	/**
	 * @param Dispatcher $dispatcher
	 */
	protected function _initTwig(Dispatcher $dispatcher)
	{
		$config = Application::app()->getConfig();
		$dispatcher->setView(new Twig(APP_PATH.'views', $config->twig->toArray()));
	}
}
```

Add to `application.ini`:

```ini
[product]

;app
application.view.ext = twig

;twig
twig.cache = APP_PATH "../cache"

[devel : product]

;twig
twig.debug = true
```

## License

MIT license