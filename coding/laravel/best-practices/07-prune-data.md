# Prune Data

Add prunable trait to Laravel model, set the prunable method to select the
prunable dates.

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Prunable;

class Log extends Model
{
    use Prunable;

    public function prunable()
    {
        return $this->where('created_at', '<', now()->subDays(30));
    }
}
```

## Add To Scheduler

Add the following line to the scheduler to run prunable daily.

```php
<?php

use Illuminate\Support\Facades\Schedule;

Schedule::command('model:prune')->daily();
```
