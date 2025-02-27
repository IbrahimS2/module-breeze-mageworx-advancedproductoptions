# Mageworx Advanced Product Options Integration

## Description

This extension does its best to integrate all storefront features of Advanced Product Options extension form Mageworx vendor.

## Required patches

`mageworx/module-optionbase/view/base/web/js/catalog/product/base.js`

```diff
@@ -50,7 +50,7 @@
         _init: function initPriceBundle() {
             var self = this;
             $(document).ready(function () {
-                $('#product-addtocart-button').attr('disabled', true);
+                $('#product-addtocart-button').prop('disabled', true);
                 // Get existing updaters from registry
                 var updaters = registry.get('mageworxOptionUpdaters');
                 if (!updaters) {
@@ -83,7 +83,7 @@
                 });

                 self.processApplyChanges();
-                $('#product-addtocart-button').attr('disabled', false);
+                $('#product-addtocart-button').prop('disabled', false);
             });
         },

```

`mageworx/module-optiondependency/view/base/web/js/dependency.js`

```diff
@@ -118,12 +118,12 @@
             if ($.inArray(optionObject.type, ['drop_down', 'multiple']) !== -1) {
                 if (optionObject.type === 'drop_down') {
                     // For dropdown - for selected select options only
-                    $('#' + option.attr('id') + ' option:selected').each(function () {
+                    $('#' + option.attr('id') + ' option:checked').each(function () {
                         self.toggleDropdown(optionObject, self.getOptionObject($(this).attr('data-option_type_id'), 'value'));
                     });
                 } else {
                     // For multiselect - for all select options
-                    var selectedMultiselectValues = $('#' + option.attr('id') + ' option:selected');
+                    var selectedMultiselectValues = $('#' + option.attr('id') + ' option:checked');
                     if (selectedMultiselectValues.length > 0) {
                         self.toggleMultiselect(optionObject, selectedMultiselectValues);
                     } else {
```

`mageworx/module-optionfeatures/view/frontend/web/js/swatches/additional.js`

```diff
@@ -320,7 +320,7 @@
                     images = $.extend(true, {}, params.options[optionId]['values'][valueId]['images']);
                 }

-                if (typeof params.$element == 'undefined' || !params.$element instanceof jQuery) {
+                if (typeof params.$element == 'undefined') {
                     return;
                 }

@@ -440,7 +440,9 @@
             clearImagesContainer: function () {
                 var params = this.options;
                 var $imagesContainer = this.getOptionGalleryContainer();
-                if (!_.isUndefined($imagesContainer) && $imagesContainer instanceof jQuery) {
-                    $imagesContainer.html('');
-                }
+                $imagesContainer.html('');
             },

             /**
```

## Installation

```bash
composer require swissup/module-breeze-mageworx-advancedproductoptions
bin/magento setup:upgrade --safe-mode=1
```
