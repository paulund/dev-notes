# Working With Third Parties

How should you structure your code when working with third parties?

I like to treat third parties as mini domains within the application code.

I will give them their own folder where I manage the services, data transfer objects
and value objects.

For example if I was going to integrate with mailchimp I would have the following
folder structure.

- Services
    - Mailchimp
      - DataTransferObject
      - ValueObject
      - MailchimpService.php
      - MailchimpClient.php

## Mailchimp Client
The `MailchimpClient.php` is used to store the auth and headers for the API.

```php
namespace App\Services\Mailchimp;

use GuzzleHttp\Client;

class MailchimpClient extends Client
{
    public function __construct(array $config = [])
    {
        parent::__construct([
            'timeout' => 30,
            'connect_timeout' => 1,
            'base_uri' => 'https://<dc>.api.mailchimp.com/3.0/',
            'headers' => [
                'Content-Type' => 'application/json',
                'Accept' => 'application/json',
                'Authorization' => 'Bearer: ' . config('services.mailchimp.secret'),
            ],
        ]);
    }
}
```
## Mailchimp Service
The service file will be used to interact with the Mailchimp API and perform operations such as sending campaigns, managing lists, and tracking analytics.

```php
namespace App\Services\Mailchimp;

class MailchimpService
{
    private $client;

    public function __construct(private readonly MailchimpClient $client)
    {
    }

    public function createCampaign(array $data): CampaignData
    {
        $response = $this->client->post('/campaigns', [
            'json' => $data,
        ]);

        return new CampaignData(json_decode($response->getBody()->getContents(), true));
    }

    // Other methods for interacting with the Mailchimp API
}
```

## Data Transfer Objects

Use Data Transfer Objects (DTOs) to encapsulate the data sent to and from the Mailchimp API.

```php
namespace App\Services\Mailchimp\DataTransferObject;

class CampaignData
{
    public function __construct(
        public readonly string $id,
        public readonly string $name,
        public readonly string $status,
    ) {
    }
}
```

## Value Objects

Use Value Objects (VOs) to represent concepts in your domain with a clear and explicit API.

```php
namespace App\Services\Mailchimp\ValueObject;

class CampaignStatus
{
    private function __construct(private readonly string $value)
    {
    }

    public static function create(string $value): self
    {
        // Validate and sanitize the value
        return new self($value);
    }

    public function getValue(): string
    {
        return $this->value;
    }
}
```
