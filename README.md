smart
=====

Smart DevNet PHP

Usage
=========
```php
 $smart = new Smart_PHP();
 $smart->spId 			= '1';
 $smart->spPassword 	= '108^5774';
 $smart->spServiceId 	= '1134';
 $smart->nonce 			= '2010082108334600001';
 $smart->creationTime 	= '2010-08-21T08:33:46Z';
 $smart->transId 		= '200903241230451000000110011000';
 $smart->accessCode 	= '12345';
 $smart->mobileNumber 	= '639395558050';
 $smart->message		= 'Hey this is a test message!';
 $response = $smart->execute();
 var_dump($response); // Response
 var_dump($smart->error_message); // show error if any
```
