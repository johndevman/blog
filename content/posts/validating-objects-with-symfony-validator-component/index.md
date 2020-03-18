+++
title = "Validating Objects With Symfony Validator Component"
date = 2020-02-01T12:30:00+01:00
author = "John Svensson"
description = "Validating objects with Symfony Validator component"
+++

By default the [Symfony Validator component](https://symfony.com/doc/current/components/validator.html) can only validate simple variables such as strings, numbers and arrays.

```php
<?php

use Symfony\Component\Validator\Constraints\Length;
use Symfony\Component\Validator\Constraints\NotBlank;
use Symfony\Component\Validator\Validation;

$validator = Validation::createValidator();
$violations = $validator->validate('Example', [
    new Length(['min' => 10]),
    new NotBlank(),
]);
```

To validate objects you need to use the ValidatorBuilder to build and return a Validator instance.

```php
<?php

use Symfony\Component\Validator\Validation;

$builder = Validation::createValidatorBuilder();
$builder->addMethodMapping('loadValidatorMetadata');

$validator = $builder->getValidator();
```

Now add a static method `loadValidatorMetadata` to your class with your constraints.

```php
<?php

use Symfony\Component\Validator\Constraints\NotBlank;
use Symfony\Component\Validator\Mapping\ClassMetadata;

class Book {
    public $name;

    public static function loadValidatorMetadata(ClassMetadata $metadata) {
        $metadata->addPropertyConstraint('name', new NotBlank());
    }
}
```

You can now validate that object.

```php
<?php

use Symfony\Component\Validator\Validation;

$builder = Validation::createValidatorBuilder();
$builder->addMethodMapping('loadValidatorMetadata');

$validator = $builder->getValidator();

$book = new Book();

$violations = $validator->validate($book);
```
