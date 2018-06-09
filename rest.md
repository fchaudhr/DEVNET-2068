# CVIM REST API Lab for Devnet-2068

## Exercise 1 - Validate REST API

* Start Postman client on the PC
* Create a `GET` **Request** with the below parameters
	* URL: `https://10.201.241.229:8445/`
	* Authorization Type: `Basic`
	* Username: `admin`
	* Password: `166d4312ec940bf2204d`

* Click `Send` on Postman
* Verify **response** of `200 OK` 
 
## Exercise 2 - Fetch HW info from CVIM

Fetch the Hardware Information from the Cisco VIM POD

* On Postman client on the PC, create a new `GET` **Request** with the below parameters

	* URL: `https://10.201.241.229:8445/v1/hwinfo`
	* Authorization Type: `Basic`
	* Username: `admin`
	* Password: `166d4312ec940bf2204d`

* Click `Send` on Postman
* Verify **response** of `200 OK` and check the JSON data for all the servers

## Exercise 3: Get Feature mapping
 
Obtain the Feature Mapping for the Software Release

* On Postman client on the PC, create a new `GET` **Request** with the below parameters

	* URL: `https://10.201.241.229:8445/v1/releasemapping/mandatory_features_mapping`
	* Authorization Type: `Basic`
	* Username: `admin`
	* Password: `166d4312ec940bf2204d`

* Click `Send` on Postman
* Verify **response** of `200 OK` and check the JSON data for features such as `networkType` supported by this release of CVIM

Append "" to the URL in Exercise#1 and send the GET request again.



## Exercise 4: DIY - Optional


#### Step1: Get the setup data yaml file from the Cisco VIM POD

* On Postman client on the PC, create a new `GET` **Request** with the below parameters

	* URL: `https://10.201.241.229:8445/v1/setupdata`
	* Authorization Type: `Basic`
	* Username: `admin`
	* Password: `166d4312ec940bf2204d`

* Click `Send` on Postman

* In the HTTP **Response**, you will find the content of **setup_data**


#### Step 2: Use and modify the Setup_data

* Copy the setup data from the HTTP response and edit the json format so that it looks like:  
`{      "jsondata": {.. .. ..} }`

	**Tip:** 

* Change  
`"jsondata": "{ \"external_lb_vip_address\": \"172.29.86.9\"`  
to  
`"jsondata": {"external_lb_vip_address": "172.29.86.9"`

So you:

* Remove the `"` before the curly bracket at the start. 
* Remove backslash `\` from the string wherever they appear


#### Step 3: offline validation

* On Postman client on the PC, create a new `POST` **Request** with the below parameters

	* URL: `https://10.201.241.229:8445/v1/offlinevalidation`
	* Content-Type: `application/json`
	* Authorization Type: `Basic`
	* Username: `admin`
	* Password: `166d4312ec940bf2204d`
	* Body:   
	
	~~~ json
	{  
	"jsondata": {.. .. ..}  
	}
	~~~

* Click `Send` on Postman
* Note down the **UUID** in the **Response**

#### Step 4:  

* To check the status of the offline validation, send a **GET** request to the following URL
`https://10.201.241.229:8445/v1/offlinevalidation/{uuid}`

  where {uuid} was received as per step 3

