# Export Profile 

Export part responsible for the technical side.
* data mapping
* external service connector(if required)
* specific configuration of a specific target system


In your export [module].

## Creating export profile class
### Domain 
New export profile should extend  Abstract Class `Ergonode\Exporter\Domain\Entity\Profile\AbstractExportProfile` in `src\Domain` folder.

```php
use Ergonode\Exporter\Domain\Entity\Profile\AbstractExportProfile;

/**
 */
class YourExportProfile extends AbstractExportProfile
{
    public function getType(): string
    {
        return 'your-type';        
    }
}
``` 

Next Create Factory to ```YourExportProfile```. This class should implement ```Ergonode\Exporter\Domain\Factory\ExportProfileFactoryInterface```

```php
use Ergonode\Exporter\Domain\Factory\ExportProfileFactoryInterface;

/**
 */
class YourExportProfileFactory extends ExportProfileFactoryInterface
{
     /**
         * @param string $type
         *
         * @return bool
         */
        public function supported(string $type): bool
        {
            return $type === 'your-type';
        }
    
        /**
         * @param ExportProfileId $exportProfileId
         * @param string          $name
         * @param array           $params
         *
         * @return AbstractExportProfile
         */
        public function create(ExportProfileId $exportProfileId, string $name, array $params = []): AbstractExportProfile
        {
            return new YourExportProfile( );
        }
}
``` 

### Infrastructure
The infrastructure of the export profile which is later required for profile selection. This class should implement ```Ergonode\Exporter\Infrastructure\Provider\ExportProfileInterface```

```php
use Ergonode\Exporter\Infrastructure\Provider\ExportProfileInterface;

/**
 */
class YourExportProfile implements ExportProfileInterface
{
    /**
     * @return string
     */
    public function getType(): string
    {
        return 'your-type';
    }

    /**
     * @param string $type
     *
     * @return bool
     */
    public function supported(string $type): bool
    {
        return $type === 'your-type';
    }
}
```

And create validator to Form. Who  should implement ```Ergonode\Exporter\Infrastructure\ExportProfile\ExportProfileValidatorStrategyInterface```

```php
use Ergonode\Exporter\Infrastructure\ExportProfile\ExportProfileValidatorStrategyInterface;

/**
 */
class Magento2ExportProfileValidatorStrategy implements ExportProfileValidatorStrategyInterface
{
    /**
     * @param string $type
     *
     * @return bool
     */
    public function supports(string $type): bool
    {
        return $type === 'your-type';
    }

    /**
     * @param array $data
     *
     * @return Constraint
     */
    public function build(array $data): Constraint
    {
        return new Collection(
            [
                'your-specific-date' => [
                    new NotBlank(),
                ],
            ]
        );
    }
}
```

In feature create and update form. 

[//]: # 

[module]: <../cookbook/new_module.md>
