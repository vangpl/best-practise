
#Kodestandard Og Refaktorering

##Variabler
⁃ Alltid engelsk.
⁃ Hungarian Notation ( https://en.wikipedia.org/wiki/Hungarian_notation )
⁃ Beskrive hva den faktisk inneholder. Ikke $test = “foo”;

###Eksempler :
```php
$strUserName ="stiandalen";
$nUserId = 10;
$arrUsers = array();
´´
  
##Variabelsjekk
⁃ Alltid sjekk variabler før bruk.
⁃ Spesielt viktig i JS. Der må det først sjekkes på om typeof !== 'undefined'

###Eksempler :
```php
$arrUsers = magicApiCall();
if(is_array($arrUsers)) {
      foreach($arrUsers as $key=>$value) {
          // Do some magic..
      }
  else {
        // Return sensible error message
  }
 }

  if (typeof nUserId !== "undefined" ) {
      // Do some magic..
  }
  else {
      // return sensible error message
  }
```

##Refaktorering
⁃ Om man ser at ny kode er tilnærmet lik en annen kode som er brukt. Lag en funksjon!
⁃ Kan funksjonen brukes av andre klasser / controllere / bundler. Lag en ny klasse slik at koden er tilgjengelig i alle klasser som den trengs.

Eksempler : Filer fra bestillkatalogcontroller.php

##ReturnVerdier til et ajax-kall
⁃ Standarisere returverdi fra ajaxkall-
- Bør inneholde en suksess-variabel som er true dersom ingenting feiler.
⁃ Bør inneholde en feilmelding dersom noe feiler.
⁃ Bør inneholde en suksess-melding dersom det er aktuelt.
⁃ Kan inneholde data i retur.

###Eksempel på php-fil i ez.

```php
$this->arrResponseReturn['success'] = $this->bSuccess;
$this->arrResponseReturn['errorMessage'] = $this->strErrorMessage;
$this->arrResponseReturn['responseMessage'] = $this>strResponseMessage;
$this->arrResponseReturn['debugMessage'] = $this->strDebugOutputMessage;
$this->arrResponseReturn['data'] = $this->data;

return new Response(json_encode($this->arrResponseReturn));

###Eksempel på ajax-kallet
```javascript
$.ajax({
  type: "POST",
  url: "/PathToUrl",
  cache: false,
  data: arrData,
  dataType: "json",
  success: function(result) {
    if( typeof result.success !== "undefined" && result.success ) {
      // Do some magic.
    }
    else {
      // output error message
      if( typeof( result.errorMessage !== 'undefined' && result.errorMessage)) {
        $('#messageContainer').html(result.errorMessage);
      } else {
        $('#messageContainer-container').html('Something went wrong');
      }
    }
  },
  error: function() {
      $('#messageContainer').html('Something went wrong');
  },timeout :  5000
});
```

##Funksjoner / Metoder
⁃ Forsøke så langt som mulig at de aldri blir lengre enn en skjermhøyde. Da bør det være mulig å refaktorere.
⁃ Minimere antallet parametre som sendes med i funksjonskallet.

##Kommentarer
⁃ Kommenter koden. Selvom det er innlysende for den som har skrevet den behøver det ikke være det for nestemann som skal se på den.

##Logging
⁃ Logging av kristiske events i en egen logifle.