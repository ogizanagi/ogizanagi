```php
class Kernel extends SymfonyKernel implements EventSubscriberInterface
{
    use MicroKernelTrait;

    public function bowtiesAction(): Response
    {
        return new Response('I wear a fez now. Fezzes are cool!');
    }

    public function dangerousAction(): Response
    {
        throw new Danger('🔥🔥 😈🐷 🔱🔺 🔥🔥');
    }

    public function onKernelException(ExceptionEvent $event): void
    {
        if ($event->getThrowable() instanceof Danger) {
            $event->setResponse(new Response("👴🏻: It's dangerous to go alone. Take this: 🗡️"));
        }
    }

    protected function configureRoutes(RoutingConfigurator $routes): void
    {
        $routes->add('bowties', '/bowties')->controller('kernel::bowtiesAction');
        $routes->add('danger', '/danger')->controller('kernel::dangerousAction');
    }

    public static function getSubscribedEvents(): array
    {
        return [KernelEvents::EXCEPTION => 'onKernelException'];
    }
}
```
