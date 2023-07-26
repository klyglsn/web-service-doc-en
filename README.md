

<img align="center" width="45%" src="logo.png">

# Web Servis Teknik Dokümanı










***Table of Contents***

| Subject | Page Number |
| -------- | ------- |
|[Introduction	](#_toc111207425) | 5|
|[General Information	](#_toc111207426) | 5|
|[Generic Cargo Integration	](#_toc111207427) |6 |
|[Cargo Services	](#_toc111207428) | 8|
|[SETDELIVERY	](#_toc111207429) | |
|[Request Parameters	](#_toc111207430) | 9|
|[Response Parameters (200 = Success)	](#_toc111207431) |14 |
|[Error Return Parameters	](#_toc111207432) |15 |
|[CANCELDELIVERY	](#_toc111207433) | 16|
|[Request Parameters	](#_toc111207434) |16 |
|[Response Parameters (200 = Success)	](#_toc111207435) | 16|
|[Error Return Parameters	](#_toc111207436) |16 |
|[TRACKDELIVERY	](#_toc111207437) | 16|
|[Request Parameters	](#_toc111207438)|16|
|[Response Parameters (200 = Success)	](#_toc111207439) | 16|
|[Error Return Parameters	](#_toc111207440) |19 |
|[CARGOMEASUREMENTUPDATE	](#_toc111207441) |19 |
|[Request Parameters	](#_toc111207442) |20 |
|[Response Parameters (200 = Success)	](#_toc111207443) | 21|
|[Error Return Parameters	](#_toc111207444) | 21|
|[GETBARCODEBYTRACKINGNUMBER	](#_toc111207445) |22 |
|[Request Parameters	](#_toc111207446) | 22|
|[Response Parameters (200 = Success)	](#_toc111207447) |22 |
|[Error Return Parameters	](#_toc111207448) |22 |
|[GETBARCODE	](#_toc111207449) |22 |
|[Request Parameters	](#_toc111207450) |22 |
|[Response Parameters (200 = Success)	](#_toc111207451) |24 |
|[Error Return Parameters	](#_toc111207452) |24 |
|[Details to Use on Services	](#_toc111207453) | 25|
|[Sendeo Status List	](#_toc111207454) |25 |
|[Sendeo List of Cities and Districts	](#_toc111207455) | 27|



***Chart List***

[Figure 1 Login Method Swagger	5****](#_toc111207456)**

[**Figure 2 List of Cargo Services	8****](#_toc111207457)

[**Figure 3 SetDelivery Request Parameters	13****](#_toc111207458)

[**Figure4 SetDelivery products array	13****](#_toc111207459)

[**Figure 5 SetDelivery Response Parameters (200 = Success)	14****](#_toc111207460)

[**Figure 6 SetDelivery Error Return Parameters	15****](#_toc111207461)

[**Figure 7 SetDelivery Exception Message	15****](#_toc111207462)

[**Figure 8 CancelDelivery Faulty Return Parameters	16****](#_toc111207463)

[**Figure 9 TrackDelivery Response Parameters (200 = Success)	17****](#_toc111207464)

[**Figure 10 TrackDelivery products.array	18****](#_toc111207465)



![](Sendeo.3322e350-ddf5-4917-aaa7-f67ba59aab07.002.png)

This document has been drafted for Sendeo web service integrations. <a name="ole_link19"></a><a name="ole_link18"></a>It describes the web services to be used for the integration and the access to related services.

# <a name="_toc111207426"></a>General Information

Sendeo services run via the link <https://api.sendeo.com.tr>. The link also provides the opportunity to view Swagger and service-related information. Using the services requires getting customer-based tokens. This information is taken through Token/LoginAES service on a customer basis.  TLS 1.2 must be used for Access.

![](Sendeo.3322e350-ddf5-4917-aaa7-f67ba59aab07.003.png)

<a name="_toc111207456"></a>*Figure 1 Login Method Swagger*

There is a single test account for different customers in order to ensure easy monitoring of the integration tests both by the customer and Sendeo. Once tests are successfully completed, credentials (user name and password) to be used in the live environment should be requested from our Sales Team for "go-live".

Test ACCOUNT Information

“customer”: “**TEST”**

“password”: **TesT.43e54**

# <a name="_toc111207427"></a>Generic Cargo Integration

The working principle for Sendeo cargo integration is the creation of the shipment by means pickup operation on the side of Sendeo by using the information submitted/provided by the client via web service for their outbound cargo. 

With generic Web service integration, our client can perform the integration with a unique number (referenceNo) which the client creates the barcode on its own label or by generating Sendeo carriage barcode.

A unique number (referenceNo) is generated for shipment pickup. The client can track all states of the parcels/shipments using the unique number generated. The end user can track the shipment using the parametric tracking link received from the web service.





#### *Generic Cargo Integration workflow:*

1. The client transfers their shipment-related data to Sendeo system using "setdelivery".
1. The referenceNo info available within the data transferred to the system should be available on the barcode generated 
1. Prior to physical pickup, data can be cancelled using "cancelDelivery". Job orders for which pickup has been completed cannot be cancelled. Cancellation will not be possible since carriage has been started after the shipment creation.
1. Pickup is performed by means of reading the referenceNo generated by our client at the pickup phase
1. Following the shipment creation, the status of the shipment may be tracked with "trackdelivery" method
1. For clients who have provided/submitted Quantity and Deci/Kg info, after the data is submitted through SETDELIVERY service, cargo quantity and deci info for the order which is sent through CARGOMEASUREMENTUPDATE service should be re-submitted. After SETDELIVERY, if no deci info is submitted via CARGOMEASUREMENTUPDATE service, then the operation will proceed by measuring-weighing the shipment itself.
1. You can query the cityId and districtId information of the “SetDelivery” service from the “GetCityDistricts” service. You can access the city and district codes by querying the GetCityDistricts service by the name of the province or the name of the province/district. In case the relevant service is not used, access to our province and district codes via the province-district excel list in our technical document.











# <a name="_toc111207428"></a>Cargo Services

![](Sendeo.3322e350-ddf5-4917-aaa7-f67ba59aab07.004.png)

![](Sendeo.3322e350-ddf5-4917-aaa7-f67ba59aab07.005.png)

![](Sendeo.3322e350-ddf5-4917-aaa7-f67ba59aab07.006.png)

![](Sendeo.3322e350-ddf5-4917-aaa7-f67ba59aab07.007.png)

![](Sendeo.3322e350-ddf5-4917-aaa7-f67ba59aab07.008.png)

<a name="_toc111207457"></a>*Figure 2 List of Cargo Services*

The cargo services offered for general use are above. Successful requests will return 200, failed/wrong requests will return error code 500 depending on the error message. It will return error 401 if authentication is not performed with token.

The main usage purposes of these services are briefly as follows:

SETDELIVERY

It allows customers to submit requests to Sendeo and to create Shipment/Job Order.

TRACKDELIVERY

It allows customers to submit requests to Sendeo and to track the pre-created Shipment using different data.

CANCELDELIVERY

It allows customers to cancel the pre-created Shipment/Job Order by means of submitting a request to Sendeo.

CARGOMEASUREMENTUPDATE

It allows customers to update quantity and measurement data for the pre-created Shipment/Job Order by means of submitting a request to Sendeo.

GETBARCODEBYTRACKINGNUMBER

It allows customers to get barcode using the tracking number for the pre-created Shipment/Job Order by means of submitting a request to Sendeo.
# <a name="_toc111207429"></a>SETDELIVERY
This is the service which allows customers to create shipment/job order on the side of Sendeo using specific data. It runs with POST method. Shipment/job order can be created in 6 different ways. These are:

- **DeliveryType 1:** Your Location >> Your Client: This is used for shipments which are going from Location to the Client. If there are multiple locations and separate invoice will be issued for each of these locations, then it should be managed with different users. If the invoice will be issued to the Main Client, then deliveryType=3 should be used.
- **DeliveryType 2:** Your Client >> Your Location: This is used for normal shipments from the Client to Location or for return shipment
- **DeliveryType 3:** Your Supplier etc. >> Your Client  
- **DeliveryType 4:** Re-shipment order (????)
- **DeliveryType 5:** Point of Delivery . This is used for shipments which will be delivered to Sendeo Point of Delivery
- **DeliveryType 6:** Point of Return. This is used for shipments which will be picked up from Sendeo Point of Return

Our Clients can create shipments between all units they work with. You can also receive service as delivery and return point.

With City and District info which we shared in the technical document, the shipment will be delivered to the branch office for the relevant city-district. 

Using the Ids provided to you as ID, you can target the branch info which you want to use for delivery and return point.

### <a name="_toc111207430"></a>Request Parameters


|**Parameter**|**Type**|**Mandatory/ Non-Mandatory**|**Sample Value**|**Description**|
| :- | :- | :- | :- | :- |
|DeliveryType|integer|Mandatory|1|<p>It includes the above-mentioned Type 1, 2, 3, 4, 5, 6 options. It indicates the point of collection and point of delivery of the shipment.</p><p></p>|
|ReferenceNo|string|Non-Mandatory|“sendeo-aygaz-12345”|It includes the reference number which the clients use internally. Using this value, the shipments can be tracked and return requests can be created.|
|Description|string|Non-Mandatory|“a shipment which includes a book about Sendeo”|You can use this field if you wish to enter any special information or description about the shipment.|
|Sender|string|Variable|“Ali Sendeo”|<p>It includes sender client's title. **Mandatory** for deliveryType = 2,3.</p><p>This field will **Not be filled** for deliveryType = 1.</p>|
|SenderId|string|Non-Mandatory||Client code info on the side of Sendeo. If clients registered on the side of Sendeo are not used, then relevant field should not be submitted.|
|SenderAuthority|string|Non-Mandatory|“Can Sendeo”|It indicates sender client's official/contact person.|
|SenderBranchCode|integer|Variable|123|This is the mandatory field which must be sent in the return point operation. If deliveryType = 6 is not selected, then this field **should be left empty**.|
|SenderAddress|string|Mandatory |TEST ADDRESS TEST ADDRESS|It varies depending on the deliveryType data.|
|SenderCityId|integer|Mandatory|34|The city Id to come from the City table should be entered as sender client's city.|
|SenderDistrictId|integer|Mandatory|34139|The Id to come from the District table should be entered sender client's district.|
|senderPhone|string|Variable|2121234567|<p>It indicates sender client's phone number. Ideally, the phone number is expected to be submitted with 10 digits in the form of xyz1234567.</p><p>*Either of Phone or GSM fields must be sent.*</p>|
|SenderGSM|string|Variable|5361234567|<p>It indicates sender client's GSM number. Ideally, the phone number is expected to be submitted with 10 digits in the form of 5xx1234567</p><p>*Either of Phone or GSM fields must be sent.*</p>|
|SenderEmail|string|Non-Mandatory|<sendeo@sendeo.com>|It includes sender client's e-mail address.|
|Receiver|string|Variable|“Ali Sendeo”|<p>It includes receiver client's title. **Mandatory** for deliveryType = 1,3.</p><p>**Not filled** for deliveryType = 2.</p>|
|ReceiverId|string|Variable||In case DeliveryType=2, then receiverId is mandatory |
|ReceiverAuthority|string|Non-Mandatory|“Can Sendeo”|It indicates receiver client's official/contact person.|
|ReceiverBranchCode|integer|Variable|123|This is the mandatory field which must be sent in the delivery point operation. If deliveryType = 5 is not selected, then this field **should be left empty**.|
|ReceiverAddress|string|Variable|“Kemerburgaz Caddesi Vadi İstanbul Park 7B Blok Kat:12 Sarıyer İstanbul”|<p>Receiver client's address details should be entered.</p><p>It includes receiver client's title. **Mandatory** for deliveryType = 1,3.</p><p>**Not filled** for deliveryType = 2.</p>|
|ReceiverCityId|integer|Mandatory|34|The city Id to come from the City table should be entered as receiver client's city.|
|ReceiverDistrictId|integer|Mandatory|34139|The ID to come from the District table should be entered receiver client's district.|
|receiverPhone|string|Variable|2121234567|<p>It indicates receiver client's phone number. Ideally, the phone number is expected to be submitted with 10 digits in the form of xyz1234567.</p><p>*Either of Phone or GSM fields must be sent.*</p>|
|ReceiverGSM|string|Variable|5361234567|<p>It indicates receiver client's GSM number. Ideally, the phone number is expected to be submitted with 10 digits in the form of 5xx1234567</p><p>*Either of Phone or GSM fields must be sent.*</p>|
|ReceiverEmail|string|Non-Mandatory|<sendeo@sendeo.com>|It includes receiver client's e-mail address|
|PaymentType|integer|Mandatory|1|As a standard, 1 **should be entered**.|
|CollectionType|integer|Mandatory|0|<p>It shows the payment type for cargos with payment on delivery:</p><p>0 – Cargo without payment on delivery</p><p>1 – Cash</p><p>2 – Credit Card</p>|
|CollectionPrice|double|Mandatory|0|It shows how much you will collect from the client for cargos with payment on delivery. The amount to be collected should be sent. If CollectionType value is 0, CollectionPrice should be 0.|
|dispatchNoteNumber|string|Non-Mandatory|“1a2b3-SendeO”|It covers the dispatch serial and number which you created for the client. If entry is available, it is possible to query the shipments accordingly.|
|ServiceType|integer|Mandatory|1|<p>It indicates the service type. Default 1 should be sent excluding special operations.</p><p></p>|
|barcodeLabelType|integer|Mandatory|1|<p>It indicates the printing type for the barcode label which you will get at the service return.</p><p>1 – Base64</p><p>2 – ZPL</p><p>3 – ZPLs</p><p>The detail of the shipment info received as array. While you can send this as total count/quantity, you can also send in different ways by detailing your shipments.</p><p>It is necessary that it should not be **sent** as zero (0)</p>|
|customerReferenceType|string|Non-Mandatory|A123|This is the field which allows the client to enter more details for situations where referenceNo field is not sufficient to allocate reference-based shipment as per the client business mechanism.|

<a name="_toc111207458"></a>*Figure 3 SetDelivery Request Parameters*

|products array|||||
| :-: | :- | :- | :- | :- |
|Count|Integer|Mandatory|2|It indicates the count/quantity of shipments. Count/quantity should be entered for each detail. Barcode number will be generated as many as the count total.|
|ProductCode|String|Non-Mandatory||It **should be sent** as empty string.|
|Description|string|Non-Mandatory||<p>Product-related description</p><p>It **should be sent** as empty string.</p>|
|Deci|integer|Non-Mandatory|20|<p>It includes deci info of the shipment packages. It is used together with Count. If not submitted, Sendeo Operation performs the measurement.</p><p>*If you have a single shipment, 2 packages, one being 3 deci and the other being 4 deci:*</p><p>- *count : 1, deci : 4*</p><p>- *count : 1, deci : 3*</p><p>*will be sent.*</p><p>*If single shipment is composed of two boxes of 5 deci:*</p><p>- *count : 2, deci : 5*</p><p>*will be sent.*</p>|
|Weigth|integer|Non-Mandatory||It indicates the weight of shipment parcels.|
|DeciWeight|integer|Non-Mandatory||It indicates the deci/weight value of the shipment parcels.|
|Price|double|Mandatory |||

<a name="_toc111207459"></a>*Figure4 SetDelivery products array*


### <a name="_toc111207431"></a>Response Parameters (200 = Success)


|**Parameter**|**Type**|**Sample Value**|**Description**|
| :- | :- | :- | :- |
|TrackingNumber|string|9124356789047|Tracking number of the job order/shipment created|
|TrackingUrl|string|https://sube.sendeo.com.tr/takip?ccode=111111&musref=sendeo-aygaz-12345|Tracking URL for the job order/shipment created. Once job orders are turned into a shipment, the tracking URL will be active.|
|Barcode|string|Base64 barcode sample will return|It includes the returning Base64 barcode for the request submitted with barcodeLabelType 1.|
|BarcodeZpl|string|ZPL barcode sample will return|It includes the returning ZPL Zebra barcode for the request submitted with barcodeLabelType 2.|
|BarcodeNumbers|string|Barcode number for the shipment|It includes barcode numbers for the shipment.|
|BarcodeZpls|string|ZPL returns as detail array|It includes multiple ZPL of the products for the shipment returning for the request submitted with barcodeLabelType 3.|

<a name="_toc111207460"></a>*Figure 5 SetDelivery Response Parameters (200 = Success)*


### <a name="_toc111207432"></a>Error Return Parameters

|**Parameter**|**Type**|**Sample Value**|**Description**|
| :- | :- | :- | :- |
|RequestId|string|f1d6d72c-643b-4a40-a142-d61b47f01187|Unique ID on the system for the request submitted|
|ExceptionMessage|string|102|The message returning with regard to the error code of the submitted request|

<a name="_toc111207461"></a>*Figure 6 SetDelivery Error Return Parameters*

|**ExceptionMessage**|**statusCode**|**Action**|
| :- | :-: | :- |
|Delivery Reference No Cannot be Null.|102|ReferenceNo field must be filled|
|Wrong Delivery Type|103|DeliveryType info should not be sent other than what is provided in the document|
|Products Cannot Be Null|104|Product Array should not be empty |
|Payment Type Is Not Correct.|411|PaymentType field must be filled|
|Collection Type Not Found|105|CollectionType must be provided|
|Product Price Cannot Be Zero.|410|In case CollectionType is different than zero, productPrice should be different than zero|
|Receiver Cannot Be Null|201|Receiver field must be filled|
|Please Enter Receiver Phone or GSM Number|207|Receiver's phone or GSM field must be filled|
|Receiver Address Cannot Be Null|202|Receiver address field must be filled|
|Receiver District Id Cannot Be Zero.|203|Receiver's district ID field must be filled|
|Receiver City Id Cannot Be Zero.|204|Receiver City Code field must be filled|
|Receiver City Id is Not Correct.|205|Receiver city code is incorrect|
|Receiver District Id is Not Correct.|206|Receiver's district ID is incorrect|
|Sender Cannot Be Null.|301|Sender field must be filled as per the deliveryType|
|Please Enter Sender Phone or GSM Number|307|Sender's phone or GSM field must be filled as per the deliveryType|
|Sender Address Cannot Be Null.|302|Sender's address field must be filled as per the deliveryType|
|Sender District Id Cannot Be Zero.|303|Sender's district field must be filled as per the deliveryType|
|Sender City Id Cannot Be Zero.|304|Sender City code must be filled as per the deliveryType|
|Sender City Id is Not Correct.|305|Sender's city code is incorrect as per the deliveryType|
|Sender District Id is Not Correct.|306|Sender's district code is incorrect as per the deliveryType|

<a name="_toc111207462"></a>*Figure 7 SetDelivery Exception Message*
# <a name="_toc111207433"></a>CANCELDELIVERY
The shipment may be cancelled using the tracking number (trackingNumber) or Reference Number (referenceNo) entered by the clients. The products not received by Sendeo cannot be cancelled. Delivered shipments **cannot be cancelled.** (In case of status 101, shipment can be cancelled)

It runs with POST method.
### <a name="_toc111207434"></a>Request Parameters
It gets trackingNumber or referenceNo parameters as a query string through URL.
### <a name="_toc111207435"></a>Response Parameters (200 = Success)
When cancellation is successful, the response will return IsSuccess = true.

### <a name="_toc111207436"></a>Error Return Parameters

|**ExceptionMessage**|**statusCode**|**Action**|
| :- | :-: | :-: |
|Please Enter Tracking Number or Reference Number.|701|TrackingNumber or referenceNumber must be submitted for cancellation|
|No Delivery Found.|702|The job order for which a cancellation request is sent was not found in the system. |
|You Cannot Cancel , Delivery Has Already Sent.|703|Cancellation is not possible because pickup is in progress|

<a name="_toc111207463"></a>*Figure 8 CancelDelivery Faulty Return Parameters*

# <a name="_toc111207437"></a>TRACKDELIVERY
It will return details of the shipment. The shipment can be tracked using the trackingNo the client has or the referenceNo entered by the client.

It runs with GET method.
### <a name="_toc111207438"></a>Request Parameters
It gets trackingNumber or referenceNo parameters as a query string through URL.


### <a name="_toc111207439"></a>Response Parameters (200 = Success)


|**Parameter**|**Type**|**Sample Value**|**Description**|
| :- | :- | :- | :- |
|TrackingNo|integer|912345678|Tracking number of the job order/shipment created|
|ReferenceNo|string|sendeo-aygaz-12345|The reference number identified on the side of the client for the job order/shipment created|
|SendDate|string|02/02/2022 16:55:03|The date when the created job order/shipment is sent|
|Receiver|string|Ali Sendeo|Receiver info|
|CargoAmount|double|10\.03|Pricing info for the shipment (including VAT)|
|Sender|string|Ali Sendeo|Sender info|
|State|integer|105|Unique ID for the status which indicates the status of the shipment|
|StateText|string|Loaded to Line Vehicle|Information about the status of the shipment|
|UpdateDate|string|04/02/2022 10:00:27|Update date for the last status change of the shipment|
|DeliveryDescription|string|TM Line Loading|Information about the process of the shipment|
|DealerId|integer|851|Unique ID of Sendeo receiving branch for the shipment|
|DeciWeight|integer|20|Shipment deci/kg|
|Quantity|integer|1|The number of packages within the shipment|
|TotalPrice|double|8\.50|Cargo price for the shipment|
|departureBranchName|string|İLKADIM DM|Name of the sending Sendeo branch for the shipment|
|ArrivalBranchName|string|DÖRTYOL DN|Arrival Sendeo branch name for the shipment|
|deliveryPlannedDate|string|2022-02-04T16:55:17.3590867+03:00|Planned delivery date|

<a name="_toc111207464"></a>*Figure 9 TrackDelivery Response Parameters (200 = Success)*



|products array|||||
| :-: | :- | :- | :- | :- |
|Count|integer|2|The number of packages within the shipment||
|ProductCode|string|kitapSendeo|Product code for stock shipments||
|Description|string|3 books|Product description for stock shipments||
|Price|double|8\.50|Package price for the shipment||
|StockCount|||||
|MinStockCount|||||

<a name="_toc111207465"></a>*Figure 10 TrackDelivery products.array*



|statusHistories array|||||
| :-: | :- | :- | :- | :- |
|StatusDate|integer|2|Status dates for the status history of the shipment||
|StatusId|string|106|Unique ID of the status change for the shipment||
|Status|string|TM Line Unloading|Name of the status change for the shipment||
|Description|string|TM Line Unloading|Detail of the status change for the shipment||

### <a name="_toc111207440"></a>Error Return Parameters
##
## <a name="_toc111207441"></a>CARGOMEASUREMENTUPDATE
The service which is used to update the measurement values of the pre-created shipment using SETDELIVERY method It will return details of the shipment. The shipment can be tracked using the trackingNo the client has or the referenceNo entered by the client.

It runs with POST method.



### <a name="_toc111207442"></a>Request Parameters


|**Parameter**|**Type**|**Mandatory/ Non-Mandatory**|**Sample Value**|**Description**|
| :- | :- | :- | :- | :- |
|customerReferenceNo|string|Mandatory|1|It includes the above-mentioned Type 1, 2, 3, 4, 5, 6 options. It indicates the point of collection and point of delivery of the shipment.|



|measurements array|||||
| :-: | :- | :- | :- | :- |
|quantity|integer|Mandatory|2|It allows updating the number of packages within the shipment|
|Width|integer|Mandatory|15|0 can be sent|
|Length|integer|Mandatory|10|0 can be sent|
|Height|Integer|Mandatory|20|0 can be sent|
|Deci|double|Mandatory|2\.0||
|weight|double|Mandatory|1\.0|0\.0 can be sent|

### <a name="_toc111207443"></a>Response Parameters (200 = Success)
When measurement update is successful, the response will return IsSuccess = true.
### <a name="_toc111207444"></a>Error Return Parameters


## <a name="_toc111207445"></a>GETBARCODEBYTRACKINGNUMBER
The service which helps get the barcode for the shipment created before with SETDELIVERY method using the tracking number with SETDELIVERY method.

It runs with POST method.

### <a name="_toc111207446"></a>Request Parameters


|**Parameter**|**Type**|**Mandatory/ Non-Mandatory**|**Sample Value**|**Description**|
| :- | :- | :- | :- | :- |
|trackingNumber|integer|Mandatory|912345678|Tracking number of the shipment whose barcode is requested|
###

### <a name="_toc111207447"></a>Response Parameters (200 = Success)


|**Parameter**|**Type**|**Sample Value**|**Description**|
| :- | :- | :- | :- |
|barcode|string||It includes the barcode of the shipment.|

### <a name="_toc111207448"></a>Error Return Parameters
# <a name="_toc111207449"></a>GETBARCODE
The service which helps get the barcode for the shipment pre-created with SETDELIVERY METHOD using barcode type and reference number.

It runs with POST method.

### <a name="_toc111207450"></a>Request Parameters


|**Parameter**|**Type**|**Mandatory/ Non-Mandatory**|**Sample Value**|**Description**|
| :- | :- | :- | :- | :- |
|barcodeLabelType|integer|Mandatory|1|It indicates the barcode type which is requested to get.|
|referenceNo|integer|Mandatory|8880000793512692|It is the shipment reference number.|
###

### <a name="_toc111207451"></a>Response Parameters (200 = Success)


|**Parameter**|**Type**|**Sample Value**|**Description**|
| :- | :- | :- | :- |
|TrackingNumber|string|912345678|Tracking number of the job order/shipment created|
|ShipmentId|int|122425|Shipment Id|
|TrackingUrl|string|https://sube.sendeo.com.tr/takip?ccode=111111&musref=sendeo-aygaz-12345|Tracking URL for the job order/shipment created. Once job orders are turned into a shipment, the tracking URL will be active.|
|Barcode|string|Base64 barcode sample will return|It includes the returning Base64 barcode for the request submitted with barcodeLabelType 1.|
|BarcodeZpl|string|ZPL barcode sample will return|It includes the returning ZPL Zebra barcode for the request submitted with barcodeLabelType 2.|
|BarcodeNumbers|string|Barcode number for the shipment|It includes barcode numbers for the shipment.|
|BarcodeZpls|string|ZPL returns as detail array|It includes multiple ZPL of the products for the shipment returning for the request submitted with barcodeLabelType 3.|

### <a name="_toc111207452"></a>Error Return Parameters

|**exceptionMessage**|**statusCode**|**Action**|
| :- | :-: | :-: |
||||
||||



# <a name="_toc111207453"></a>Details to Use on Services

### <a name="_toc111207454"></a>Sendeo Status List

|**statusId**|**statusName**|**statusDescription**|
| :-: | :- | :- |
|101|Cargo Shipping Order Received|Job Order Received|
|102|Document Issued|Shipment Created|
|103|Branch TM Loading|Departed from the Departure Branch|
|104|TM Branch Unloading|At the Point of Delivery|
|105|TM Line Loading|Loaded to Line Vehicle|
|106|TM Line Unloading|Unloaded from Line Vehicle|
|107|TM Branch Loading|Loaded to the Branch from Transfer Center|
|108|Branch TM Unloading|Unloaded to the Transfer Center|
|109|Branch Distribution Loading|Departed for Distribution|
|110|Branch Distribution Unloading|Branch Distribution Unloading|
|111|Delivered|Delivered|
|112|Receiver Phone Incorrect|Receiver Phone Incorrect|
|113|Return Request|Return Request|
|114|Receiver Absent at the Address|Receiver Absent at the Address|
|115|Receiver Address Incorrect|Receiver Address Incorrect|
|117|Damaged Shipment|Damaged Shipment|
|118|Lost Cargo|Lost Cargo|
|119|Transfer|Transfer|
|120|Short Delivery|Short Delivery|
|121|Out of Distribution Area|Out of Distribution Area|
|122|Payment Type Rejected|Payment Type Rejected|
|123|Delivery on Appointment|Delivery on Appointment|
|124|Mobile Distribution Region|Mobile Distribution Region|
|125|Short Cargo|Short Cargo|
|126|Routing|Routing|
|127|Line Vehicle Delay|Line Vehicle Delay|
|128|Adverse Weather Conditions|Adverse Weather Conditions|
|129|Carriage Cost Found Expensive|Carriage Cost Found Expensive|
|130|Not Delivered|Not Delivered|
|131|Returned Shipment|Returned Shipment|
|132|Measurement / Weighing|Measurement / Weighing Completed|
|133|Cancelled Shipment|Cancelled Shipment|
|134|Return Approved|Return Request Approved|
|135|Job Order Return|Shipment Returned with Job Order|
|136|Return Rejected|Rejected by client|
|137|Pick-up on Appointment|Client Scheduled an Appointment|
|138|Shipment Received|Shipment Received|
|139|Cancel Job Order|Cancel Job Order|
|140|Courier on Delivery|Courier on Delivery|
|141|Debited by Courier|Debited by Courier|
|142|Courier Released from Debit|Courier releases from debit|
|143|Lost Cargo|Lost Cargo Job Order|
|151|Delivery as a Return|Delivery as a Return|


### <a name="_toc111207455"></a>Sendeo List of Cities and Districts

List of cities and districts can be found in the Excel file attached.


[Cities and Districts List](il-ilce.xlsx)

Examples of Delivery Types 1 , 2 and 3

\*\*It will reflect areas according to customer needs. Shared as an example.

[Deliverytype 1 Sample](Deliverytype1.txt)


[Deliverytype 3 Sample](Deliverytype3.txt)

