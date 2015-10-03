#JavaScript

##Tydelig kobling mellom JS og DOM
Vi bruker kun `id` som kobling hvis elementet må være unikt og ha en `id` fra før. F.eks. hvis vi forfiner lokalnavigasjon med `#hash`-lenke.

Ellers bruker vi klasse eller data-attributter for å koble DOM til javascriptkoden. Hvis vi bruker en klasse skal den alltid prefixes med `js`. Hvis man ikke følger dette er det veldig vanskelig å refaktorere CSS-kode uten at dette kan påvirke JavaScript-koden også. På samme måte skal man aldri bruke klasser med `js`-prefix til å style med CSS.

```html
<a class="js-share">Del meg</a>
<button data-action="close">Lukk</button>
```

```javascript
$('.js-share').click(function(e) {
  e.preventDefault();

  // Åpne delevindu
});

$('button[data-action=close]').click(function() {
  e.preventDefault();

  // Lukk vindu
});
```

##Isolér kode fra global scope
Ved å pakke inn koden på denne måten isolerer vi koden fra global scope og overskriver ingen andre globale variabler. Vi binder her også `jQuery` til `$` slik at vi kan bruke `$` som referanse til jQuery selv om `$` kanskje er mappet til et annet bibliotek globalt.

Vi bruker alltid `"use strict"` slik at vi får beskjed om potensielle problemer med koden i stedet for at den feiler stille.

```javascript
(function($) {
  'use strict';

  $(document).ready(function(){

    // SOCIAL MEDIA
    $( '.js-share' ).click( function( e ){
      e.preventDefault();
      window.open(
        $( this ).attr( 'href' ),
        'share-box__link',
        'width=626,height=436'
      );
    });

  });

})(jQuery);
```

##Bygg koden som et selvstendig objekt
Ved å skrive koden på denne måten kan man enkelt lage flere selvstendige instanser av objektene. Man kan også enkelt lage en jQuery-funksjon basert på den.

```javascript
// Use code bellow to create a simple jQuery plugin
// Create code to run for each element match where options are kept separately for each instance
// Replace 'Example/mainExample ...' in the example with a more relevant name

(function($) {
  'use strict';
  
  function Example(el, options) {

    // Bind element argument to object element
    this.el = el;

    // Extends defaults-options with user defined options
    this.options = $.extend( this.defaults, options || {} );

    // Let the fun begin!
    this._init();
  };

  Example.prototype = {
    // Default options
    // Override in call "$(selector).example({option1: value1, option2: value2});" or "new mainExample($(selector,{option1: value1, option2: value2});"
    defaults : {
      //optionName: 'option value'
    },

    _init : function() {
      // Var to keep tab this-reference
      // Var shortcut to element
      var example = this,
          exampleElm = $(courseList.el);

      // Add code to run on init
      // Add and call additional methods to abstract code and make it more readable
      // For example:
      // example._otherMethod();
      
    },
    _otherMethod : function() {
      var example = this;
      
      // Add method code here
      // Add as many other methods as necessary to keep code simple and efficient

    }

  };

  // Add object to global namespace
  // This makes it accessible outside the function
  // We can prefix this value to prevent name conflicts
  window.mainExample = Example;
})(jQuery);

// Create a jquery method which creates a new mainExample object for each instance
(function($) {
  'use strict';

  $.fn.example = function( options ) {
    return this.each( function() {
      new mainExample($(this), options);
    });
  };
})(jQuery);

// Call the function on one or more selectors
$('.example').example();

// You can also use the function without jQuery
new mainExample( document.querySelector( '.example' ) );
```