Smart DevNet PHP, Orignal Code from Smart PHP SDK
==============================


The code
==============================
```php
<?php 
class Smart_PHP
{
 //Configure parameters for headers and request body
	public $spId = ''; //SP ID
	public $spPassword = ''; //SP's encrypted password
	public $spServiceId = ''; //SP's service id
	public $nonce = '';// Nonce provided. Tailored for your password, use 2010082108334600001
	public $creationTime = '';// Creation Time provided. Tailored for your password, use 2010-08-21T08:33:46Z
	public $transId = '';// TransId provided. e.g (200903241230451000000110011000)
	public $accessCode = ''; //SP's dedicated access code
	public $mobileNumber = ''; // Mobile number who will receive the message
	
	public $domainURL = '';
	public $postJSONRequest = '';
	
	public $message = '';
	
	public $requestHeaders = '';
	
	public $smartServerCert = 'npwifi.smart.com.ph.crt'; //certificate file (see certificate name: \npwifi.smart.com.ph.crt) must be on the same directory of this class
	
	public $restSendSMS = '';
	
	public $error_message = '';
	
	/*
	 *
	 *
	 Usage
	 
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
	 
	*/
	
	// --------------------------------------------------------------------
	
	function __construct()
	{
		$this->set_url();
		$this->json_request();
		$this->request_headers();
		$this->curl_prepare();
	}
	
	// --------------------------------------------------------------------
	
	function set_url()
	{
		$this->domainURL = 'https://npwifi.smart.com.ph/1/smsmessaging/outbound/' .$this->accessCode. '/requests';
	}
	
	// --------------------------------------------------------------------
	
	function json_request()
	{
		// json request
		$this->postJSONRequest = '{"outboundSMSMessageRequest":{"address":["tel:' .$this->mobileNumber. '"],"senderAddress":"' .$accessCode. '","outboundSMSTextMessage":{"message":"'.$this->message.'"}}}';	
	}
	
	// --------------------------------------------------------------------
	
	function request_headers()
	{
		//define all the request headers to be posted
		$this->requestHeaders = array('Content-Type: application/json', 
								'Accept: application/xml', 
								'Authorization: WSSE realm="SDP",profile="UsernameToken"', 
								'X-WSSE: UsernameToken Username="' .$this->spId. '",PasswordDigest="' .$this->spPassword. '",Nonce="' .$this->nonce. '", Created="' .$this->creationTime. '"', 
								'X-RequestHeader: request TransId="' .$this->transId. '",ServiceId="' .$this->spServiceId.'"');
	}
	
	// --------------------------------------------------------------------
	
	function curl_prepare()
	{
		//initialize curl (create curl resource)
		$this->restSendSMS = curl_init(); 
		
		//set the domain and other options
		curl_setopt($this->restSendSMS, CURLOPT_URL, $this->domainURL);   
		curl_setopt($this->restSendSMS, CURLOPT_CONNECTTIMEOUT, 10); 
		curl_setopt($this->restSendSMS, CURLOPT_TIMEOUT, 10); 
		curl_setopt($this->restSendSMS, CURLOPT_RETURNTRANSFER, true);
	
		//load the smart server certificate
		curl_setopt($this->restSendSMS, CURLOPT_SSL_VERIFYPEER, true);
		
		
		curl_setopt($this->restSendSMS, CURLOPT_CAINFO, getcwd(). $this->smartServerCert);
		
		//define the post fields and the post headers 
		curl_setopt($this->restSendSMS, CURLOPT_POST, true); 
		curl_setopt($this->restSendSMS, CURLOPT_POSTFIELDS, $this->postJSONRequest); 
		curl_setopt($this->restSendSMS, CURLOPT_HTTPHEADER, $this->requestHeaders);
	}
	
	// --------------------------------------------------------------------
	
	function execute()
	{
		$curlRespose = curl_exec($this->restSendSMS);
		
		$this->error_message = curl_error($this->restSendSMS); 
		
		return $curlRespose;
	}
	
	// --------------------------------------------------------------------
	
	function error()
	{
		return $this->error_message;
	}
}

?>

```


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
