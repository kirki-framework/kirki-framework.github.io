---
layout: doc
title: settings
subtitle: Define the name of your settings for your field.
tags: [arguments]
---

`settings` is a **required** argument for any field. The value entered here will be used to store the settings in the database.

If for example you create a field using something like this:

```php
new \Kirki\Field\Color( [
    'settings' => 'body_background_color',
    'label'    => esc_html__( 'Background Color', 'textdomain' ),
    'section'  => 'my_section',
    'default'  => '#0088CC',
] );
```
then in your theme you can access that value using:

```php
$color = get_theme_mod( 'body_background_color', '#0088CC' );
```
