---
layout: doc
title: active_callback
subtitle: Control whether the control will be active or not
tags: [arguments]
---

You can use the `active_callback` argument to define if you want to hide or display a control under conditions.

The default WordPress Customizer implementation for `active_callback` allows you to define a callable function or method (see the [Customizer API Handbook](https://developer.wordpress.org/themes/advanced-topics/customizer-api/#contextual-controls-sections-and-panels) for details).


Kirki extends the `active_callback` argument to allow defined field dependencies:

```php
'active_callback' => [
	[
		'setting'  => 'checkbox_demo',
		'operator' => '==',
		'value'    => true,
	]
],
```
It is formatted as an array of arrays so you can add multiple dependencies.

* `setting`: you can use the `settings` argument of the controller-field.
* `operator`: you can use any of the following:
  * `===` : uses PHP's `===` to evaluate the value
  * `==`, `=`, `equal`, `equals` : uses PHP's `==` to evaluate the value
  * `!==` : uses PHP's `!==` to evaluate the value
  * `!=`, `not equal` : uses PHP's `!==` to evaluate the value
  * `>=`, `greater or equal`, `equal or greater` : uses PHP's `>=` to evaluate the value
  * `<=`, `smaller or equal`, `equal or smaller`: uses PHP's `<=` to evaluate the value
  * `>`, `greater` : uses PHP's `>` to evaluate the value
  * `<`, `smaller` : uses PHP's `<` to evaluate the value
  * `contains`, `in` : If the context is an array then we'll check if the value defined exists in the array (using PHP's [`in_array()`](http://php.net/manual/en/function.in-array.php) function. If on the other hand the context is a string then we'll check if the string contains the value defined using PHP's [`strpos()`](http://php.net/manual/en/function.strpos.php) and checking not the position of the string but whether the result returns `false` or not (using `===` for safety).
* `value`: the value of the controller-field against which all comparisons and checks will be performed to determin if the field should be visible or not.

In the example below we're first creating a section and then we add 3 fields to our customizer: a checkbox, a radio and finally a text control.

The text control will **only** be shown if the value of the checkbox is equal to 1 **and** the value of the radio control is not equal to `option-1`.


```php
/**
 * Add section.
 */
add_filter( 'customize_register', function( $wp_customize ) {
    $wp_customize->add_section(
        'title' => esc_html__( 'My Section', 'textdomain' );
    );
});

new \Kirki\Field\Checkbox( [
	'settings'  => 'my_checkbox',
	'label'     => esc_html__( 'My Checkbox', 'textdomain' ),
	'section'   => 'my_section',
	'default'   => false,
] );

new \Kirki\Field\Radio( [
	'settings'  => 'my_radio',
	'label'     => esc_html__( 'My Radio', 'textdomain' ),
	'section'   => 'my_section',
	'default'   => 'option-1',
	'priority'  => 20,
	'choices'   => [
		'option-1' => esc_html__( 'Option 1', 'textdomain' ),
		'option-2' => esc_html__( 'Option 2', 'textdomain' ),
		'option-3' => esc_html__( 'Option 3', 'textdomain' )
	]
] );

new \Kirki\Field\Text( [
	'settings'  => 'my_setting',
	'label'     => esc_html__( 'Text Color', 'textdomain' ),
	'section'   => 'my_section',
	'default'   => esc_html__( 'my default text here', 'textdomain' ),
	'priority'  => 30,
	'active_callback'  => [
		[
			'setting'  => 'my_checkbox',
			'operator' => '===',
			'value'    => true,
		],
		[
			'setting'  => 'my_radio',
			'operator' => '!==',
			'value'    => 'option-1',
		],
	]
] );
```

The equivalent of the above rule written as a PHP function in the callback would look like this:

```php
'active_callback' => function() {
	$checkbox_value = get_theme_mod( 'my_checkbox', false );
	$radio_value    = get_theme_mod( 'my_radio', 'option-1' );

	if ( true === $checkbox_value && 'option-1' !== $radio_value ) {
		return true;
	}
	return false;
},
```
or written a lot shorter:

```php
'active_callback' => function() {
	return true === get_theme_mod( 'my_checkbox', false ) && 'option-1' !== get_theme_mod( 'my_radio', 'option-1' );
},
```

The benefit however of using the field-dependencies using Kirki instead of using a PHP function is that `active_callbacks` are applied via JavaScript instead of PHP, so changes are instant.
