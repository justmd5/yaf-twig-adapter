# yaf-twig-adapter
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fjustmd5%2Fyaf-twig-adapter.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fjustmd5%2Fyaf-twig-adapter?ref=badge_shield)


Twig Adapter for Yaf PHP Framework with namespace enabled.


## Installation

You can install via Composer.

At first create `composer.json` file:

```json
{
	"require": {
		"justmd5/yaf-twig-adapter":">=1.0"
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



## 如果你的项目中存在modules则可以参考如下:

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
	  public function _initTwig(Dispatcher $dispatcher)
        {
            if (REQUEST_METHOD !== 'CLI') {
                $config = Application::app()->getConfig();
                $modules_names = explode(',', $config->application->modules);
                $paths = [ROOT_PATH . '/' . APP_NAME . '/views'];
                array_walk($modules_names, function ($v) use (&$paths) {
                    if (is_dir(ROOT_PATH . '/' . APP_NAME . '/modules/' . $v . '/views')) {
                        array_push($paths, ROOT_PATH . '/' . APP_NAME . '/modules/' . $v . '/views');
                    }
                });
                $dispatcher->setView(new Twig($paths, isset($config->twig) ? $config->twig->toArray() : []));
            }

        }
}

```
Add to `application.ini`:

```ini
[product]

;app
;此处填写你的modules,多modules","隔开
application.modules = "Index,Log,Demo"
application.view.ext = twig

;twig
twig.cache = APP_PATH "../cache"
[devel : product]
;twig
twig.debug = true
```

## License

MIT license

[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fjustmd5%2Fyaf-twig-adapter.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fjustmd5%2Fyaf-twig-adapter?ref=badge_large)