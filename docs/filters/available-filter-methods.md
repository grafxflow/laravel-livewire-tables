---
title: Available Filter Methods
weight: 6
---

The following methods are available on the filter object.  These are "filter-specific" methods.

----

## setFilterPillTitle

By default, the filter pill title is the filter name, but you can make it whatever you want:

```php
SelectFilter::make('Active')
    ->setFilterPillTitle('User Status')
```

## setFilterPillValues

If you have numeric, or generated keys as your filter option values, they probably don't look too nice in the filter pill. You can set the values to be displayed in the filter pill:

```php
SelectFilter::make('Active')
    ->setFilterPillTitle('User Status')
    ->setFilterPillValues([
        '1' => 'Active',
        '0' => 'Inactive',
    ])
    ->options([
        '' => 'All',
        '1' => 'Yes',
        '0' => 'No',
    ])
```

Now instead of `Active: Yes` it will say `User Status: Active`

## hiddenFromMenus

Hide the filter from both the filter popover and the filter slide down.

```php
SelectFilter::make('Active')
    ->hiddenFromMenus()
```

## hiddenFromPills

Hide the filter from the filter pills when applied.

```php
SelectFilter::make('Active')
    ->hiddenFromPills()
```

## hiddenFromFilterCount

Hide the filter from the filter count when applied.

```php
SelectFilter::make('Active')
    ->hiddenFromFilterCount()
```

## hiddenFromAll

Hide the filter from the menus, pills, and count.

```php
SelectFilter::make('Active')
    ->hiddenFromAll()
```

## notResetByClearButton

By default the `clear` button will reset all filters to their defaults. You can prevent this on a specific filter by using this method.

```php
SelectFilter::make('Active')
    ->notResetByClearButton()
```
                                
## setFilterSlidedownRow

This method applies only when using the Slide Down approach to filter display.
By default the filters will be displayed in the order that they are listed in the filters() method.  This method allows you to specify the row that the filter will be listed.  When multiple filters are placed on the same row, and a mobile device is used, then the first filter listed will "win" that row.
You may use either a string or an integer to pass to this method, and it can be used in conjunction with setFilterSlidedownColspan

```php
SelectFilter::make('Active')
    ->setFilterSlidedownRow(1)
```

## setFilterSlidedownColspan

This method applies only when using the Slide Down approach to filter display.
By default each filter will take up one column, with the number of columns determined by the size of the screen, this ranges from 1 on a mobile device, to a maximum of 5 on a large display.  This method allows you to specify the number of columns that the filter should span.  It will span the number of columns specified, up to the number of columns available (depending on screen size).
You may use either a string or an integer to pass to this method, and it can be used in conjunction with setFilterSlidedownRow

```php
DateFilter::make('Date')
    ->config([
        'min' => '2020-01-01',
        'max' => '2021-12-31',
    ])
    ->setFilterSlidedownColspan('2')
```

## setFilterPillBlade

Set a blade file for use in displaying the filter values in the pills area.  You can use this in conjunction with setFilterPillValues() to prettify your applied filter values display.  You will receive two properties ($filter) containing the filter instance, and ($value) containing the filter value.

```php
SelectFilter::make('Active')
    ->setFilterPillBlade('path.to.blade')
```

Example blade:
```php
@aware(['component'])
@props(['filter'])

<span wire:key="{{ $component->getTableName() }}-filter-pill-{{ $filter->getKey() }}"
    class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium leading-4 bg-indigo-100 text-indigo-800 capitalize dark:bg-indigo-200 dark:text-indigo-900"
>
    {{ $filter->getFilterPillTitle() }} - ({{ $filter->getFilterPillValue($value) }})

    <button
        wire:click="resetFilter('{{ $filter->getKey() }}')"
        type="button"
        class="flex-shrink-0 ml-0.5 h-4 w-4 rounded-full inline-flex items-center justify-center text-indigo-400 hover:bg-indigo-200 hover:text-indigo-500 focus:outline-none focus:bg-indigo-500 focus:text-white"
    >
        <span class="sr-only">@lang($this->getLocalisationPath.'Remove filter option')</span>
        <svg class="h-2 w-2" stroke="currentColor" fill="none" viewBox="0 0 8 8"><path stroke-linecap="round" stroke-width="1.5" d="M1 1l6 6m0-6L1 7" /></svg>
    </button>
</span>
```

## setCustomFilterLabel

Set a custom blade file for the filter's label.  This will be used in both the Pop-Over and SlideDown filter displays, you should therefore ensure that you cater for the different filter layouts.

```php
SelectFilter::make('Active')
    ->setCustomFilterLabel('path.to.blade')
```

You will receive several properties to your blade, explained here:
- $filter (the filter instance)
- $filterLayout ('slide-down' or 'popover')
- $tableName (the table name)
- $isTailwind (bool - is theme Tailwind)
- $isBootstrap (bool - is theme Bootstrap 4 or Bootstrap 5)
- $isBootstrap4 (bool - is theme Bootstrap 4)
- $isBootstrap5 (bool - is theme Bootstrap 5)
- $customLabelAttributes (array -> any customLabel attributes set using setFilterLabelAttributes())

Example label blade:
```php
@props(['filter', 'filterLayout' => 'popover', 'tableName' => 'table', 'isTailwind' => false, 'isBootstrap' => false, 'isBootstrap4' => false, 'isBootstrap5' => false, 'customLabelAttributes' => []])


<label for="{{ $tableName }}-filter-{{ $filter->getKey() }}"

    {{
        $attributes->merge($customLabelAttributes)
            ->class(['block text-sm font-medium leading-5 text-gray-700 dark:text-white' => $isTailwind && $customLabelAttributes['default'] ?? true])
            ->class(['d-block' => $isBootstrap && $filterLayout == 'slide-down' && $customLabelAttributes['default'] ?? true])
            ->class(['mb-2' => $isBootstrap && $filterLayout == 'popover' && $customLabelAttributes['default'] ?? true])
            ->except('default')
    }}

>
    {{ $filter->getName() }}
</label>
```

## setFilterLabelAttributes

### Old Method (Still Supported)
Set custom attributes for a Filter Label.  At present it is recommended to only use this for "class" and "style" attributes to avoid conflicts.

By default, this replaces the default classes on the Filter Label wrapper, if you would like to keep them, set the default flag to true.

```php
TextFilter::make('Name')
    ->setFilterLabelAttributes(
        [
            'class' => 'text-xl', 
            'default' => true,
        ]
    ),
```

### New Method (Recommended)
Set custom attributes for a Filter Label.  At present it is recommended to only use this for "class" and "style" attributes to avoid conflicts.

By default, this replaces the default classes on the Filter Label wrapper, if you would like to keep them, set the default flag to true.

```php
TextFilter::make('Name')
    ->setLabelAttributes(
        [
            'class' => 'text-xl', 
            'default' => true,
        ]
    ),
```

## setInputAttributes
Allows for customising the attributes that will apply to the input field for the filter.

By default, this replaces the default classes on the Filter Input, if you would like to keep them, set the default-styling and/or default-colors flags to true.

### TextFilter Example
The following would:
- Set a maxlength of 75
- Set a placeholder of "Enter a Name"
- Replace the default colors
- Retain the default styling (e.g. rounding/shadow)

```php
public function filters(): array
{
    return [
        TextFilter::make('Name')
        ->setInputAttributes([
            'maxlength' => '75',
            'placeholder' => 'Enter a Name',
            'class' => 'text-white bg-red-500 dark:bg-red-500',
            'default-colors' => false,
            'default-styling' => true,
        ]),
    ];
}
```

### NumberFilter Example
The following would:
- Set a min of 5
- Set a max of 20
- Set steps to be 0.5
- Keep the default colors & styling

```php
public function filters(): array
{
    return [
        NumberFilter::make('Age')
        ->setInputAttributes([
            'min' => '5',
            'max' => '20',
            'step' => '0.5',
            'default-colors' => true,
            'default-styling' => true,
        ]),
    ];
}
```


## setCustomView
Use a fully custom view for a filter.  This will utilise solely your view when rendering this filter.  Note that the following methods will no longer apply to a filter using this:
- setCustomFilterLabel
- setFilterLabelAttributes

```php
TextFilter::make('Name')
    ->setCustomView('text-custom-view'),
```

## Config

If the filter takes any config options, you can set them with the `config` method:

```php
 DateFilter::make('Date')
    ->config([
        'min' => '2020-01-01',
        'max' => '2021-12-31',
    ])
```

## Customising Wireable Behaviour

For the following Filters, you may customise how the input is wire:model into the Table Component:

- DateFilter (Defaults to Live)
- DateTimeFilter (Defaults to Live)
- MultiSelectDropdownFilter (Defaults to live.debounce.250ms)
- MultiSelectFilter (Defaults to live.debounce.250ms)
- NumberFilter (Defaults to Blur)
- SelectFilter (Defaults to Live)
- TextFilter (Defaults to Blur)

You may override this using the following methods, on any of the above Filter types:

### setWireBlur()
Forces the filter to use a wire:model.blur approach
```php
    TextFilter::make('Name')
    ->config([
        'placeholder' => 'Search Name',
        'maxlength' => '25',
    ])
    ->setWireBlur()
```

### setWireDefer()
Forces the filter to use a wire:model approach
```php
    TextFilter::make('Name')
    ->config([
        'placeholder' => 'Search Name',
        'maxlength' => '25',
    ])
    ->setWireDefer()
```

### setWireLive()
Forces the fitler to use a wire:model.live approach
```php
    TextFilter::make('Name')
    ->config([
        'placeholder' => 'Search Name',
        'maxlength' => '25',
    ])
    ->setWireLive()
```

### setWireDebounce(int $debounceDelay)
Allows you to pass a string to use a wire:model.live.debounce.Xms approach
```php
    TextFilter::make('Name')
    ->config([
        'placeholder' => 'Search Name',
        'maxlength' => '25',
    ])
    ->setWireDebounce(50)
```
