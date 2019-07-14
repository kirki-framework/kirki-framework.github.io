---
layout: doc
title: Legacy API
subtitle: Legacy API for themes using Kirki versions prior to v4.0
tags: [api]
---

If your theme is not using the `composer` packages and you are using the [Kirki plugin](https://wordpress.org/plugins/kirki) directly, you will have access to the **Legacy API** which is used for backwards-compatibility.

The APIs described in this document are available from the [`kirki-framework/compatibility`](https://github.com/kirki-framework/compatibility) package which is pre-installed in the Kirki plugin.

## Configuration

When you create a project in Kirki, the first thing you have to do is **create a configuration**. Configurations allow each project to use a different setup and act as identifiers so it's important you create one. Fields that belong to your configuration will inherit your config properties.

```php
Kirki::add_config( 'theme_config_id', array(
	'capability'    => 'edit_theme_options',
	'option_type'   => 'theme_mod',
) );
```

The arguments available for the `Kirki::add_config()` method are:

* `capability`: any valid WordPress capability. See the [WordPress Codex](https://codex.wordpress.org/Roles_and_Capabilities) for details.
* `option_type`: can be either `option` or `theme_mod`. We recommend using `theme_mod`. If however you choose to use `option` you need to understand how your data will be saved, and in most cases you will also have to use the `option_name` argument as well.
* `option_name`: If you're using options instead of theme mods then you can use this to specify an option name. All your fields will then be saved as an array under that option in the WordPress database.
* `disable_output`: Set to `true` if you don't want Kirki to automatically output any CSS for your config (defaults to `false`).

To create a field that will then use this configuration you can add your fields like this:
```php
Kirki::add_field( 'theme_config_id', $field_args );
```

## Adding Panels

Panels are wrappers for sections, a way to group multiple sections together. To see how to create Panels using the WordPress Customizer API please take a look at [these docs](https://developer.wordpress.org/themes/advanced-topics/customizer-api/#panels).

Using Kirki:
```php
Kirki::add_panel( 'panel_id', array(
    'priority'    => 10,
    'title'       => esc_html__( 'My Panel', 'kirki' ),
    'description' => esc_html__( 'My panel description', 'kirki' ),
) );
```

The `Kirki::add_panel()` method is nothing more than a wrapper for the WordPress customizer API and therefore follows the exact same syntax. More information on WordPress Customizer Panels can be found on the [WordPress Codex](https://developer.wordpress.org/themes/advanced-topics/customizer-api/#panels).


## Adding Sections

Sections are wrappers for controls, a way to group multiple controls together. All fields must belong to a section, no field can be an orphan. To see how to create Sections using the WordPress Customizer API please take a look at [these docs](https://developer.wordpress.org/themes/advanced-topics/customizer-api/#sections).

Using Kirki:
```php
Kirki::add_section( 'section_id', array(
    'title'          => esc_html__( 'My Section', 'kirki' ),
    'description'    => esc_html__( 'My section description.', 'kirki' ),
    'panel'          => 'panel_id',
    'priority'       => 160,
) );
```

The `Kirki::add_section()` method is nothing more than a wrapper for the WordPress customizer API and therefore follows the exact same syntax. More information on WordPress Customizer Sections can be found on the [WordPress Codex](https://developer.wordpress.org/themes/advanced-topics/customizer-api/#sections)


## Adding Controls

To add a control you can use the `Kirki::add_field()` method.

```php
Kirki::add_field( 'theme_config_id', [
	'type'        => 'color',
	'settings'    => 'color_setting_hex',
	'label'       => __( 'Color Control (hex-only)', 'kirki' ),
	'description' => esc_html__( 'This is a color control - without alpha channel.', 'kirki' ),
	'section'     => 'section_id',
	'default'     => '#0088CC',
] );
```

## Using the `kirki_fields` filter

It is also possible to use the `kirki_fields` filter to add all fields at once using an array.

```php
add_filter( 'kirki_fields', function( $fields ) {
    $fields[] = [
        'type'        => 'color',
        'settings'    => 'color_setting_hex',
        'label'       => __( 'Color Control (hex-only)', 'kirki' ),
        'description' => esc_html__( 'This is a color control - without alpha channel.', 'kirki' ),
        'section'     => 'section_id',
        'default'     => '#0088CC',
    ]
    $fields[] = [
        'type'        => 'color',
        'settings'    => 'another_setting',
        'label'       => __( 'Another Color Control', 'kirki' ),
        'description' => esc_html__( 'This is a second color control.', 'kirki' ),
        'section'     => 'section_id',
        'default'     => '#000000',
    ]
} );
```
