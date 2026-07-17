# Laravel Form Builder

Drag-and-drop form builder for Laravel + Vue 3. Define form schemas in an admin UI (`FormBuilder`), then render and collect submissions with `VForm`.

| Package                           | Purpose |
|-----------------------------------|---------|
| `dcodegroup/laravel-form-builder` | Laravel models, migrations, validation helpers |
| `@dcodegroup-au/form-builder`     | Vue components and styles |

## Requirements

- PHP 8.2+
- Laravel 11, 12, or 13

## Installation
### Backend
```bash
composer require dcodegroup/laravel-form-builder
php artisan form-builder:install
php artisan migrate
```

`form-builder:install` publishes the `create_forms_table` and `create_form_data_table` migrations when they are not already present.

### Frontend

```bash
npm install @dcodegroup-au/form-builder
```

---

## Backend quick reference

### Backend models & traits

#### `Form`

Stores the form definition (`title`, `recipients`, `status`, `published_at`, `fields`).

```php
use Dcodegroup\FormBuilder\Models\Form;

$form = Form::saveModel([
    'title' => 'Onboarding',
    'status' => 'published',
    'fields' => $request->input('data.fields'),
]);
```

#### `FormData`

Stores a filled submission (`values`, `completed_at`) morph-linked to any model via `formable`, and related to a `Form`.

#### `HasFilledForms`

Add to any Eloquent model that can have filled forms:

```php
use Dcodegroup\FormBuilder\Models\Traits\HasFilledForms;

class Job extends Model
{
    use HasFilledForms;
}

// Latest (or create) submission for a form
$formData = $job->getFormData($form, createNew: true);

// Persist values
$job->saveFormData($form, $values);
```

#### `FormValidator`

Use on a Form Request to build rules from required fields in the schema:

```php
use Dcodegroup\FormBuilder\Http\Traits\FormValidator;
use Illuminate\Foundation\Http\FormRequest;

class StoreFormSubmissionRequest extends FormRequest
{
    use FormValidator;

    public function rules(): array
    {
        return $this->getRules([
            // extra static rules...
        ]);
    }

    public function messages(): array
    {
        return $this->getRules([], isMessage: true);
    }
}
```

Rules are derived from `route('form')?->fields` when present, otherwise from `request()->input('data.fields')`.

---

## Database

**`forms`**

| Column | Notes |
|--------|--------|
| `title` | Form name |
| `recipients` | JSON (notification emails) |
| `status` | e.g. draft / published |
| `published_at` | Set when published |
| `fields` | JSON schema |

**`form_data`**

| Column | Notes |
|--------|--------|
| `formable_type` / `formable_id` | Morph to the owning model |
| `form_id` | Related `forms` row |
| `values` | JSON answers |
| `completed_at` | Nullable completion timestamp |

---

## Testing

Run package tests (if provided):

```bash
composer test
```

## Contributing

Contributions are welcome. Please open issues or pull requests and follow repository conventions.

## License

MIT — see LICENSE.md for details.
