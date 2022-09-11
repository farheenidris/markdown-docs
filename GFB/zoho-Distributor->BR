# Zoho-Distributor->BR-Distributor

### Feature

- [x] sync every 3 hours(you can change it)
- [x] Fetch Zoho Branches and then update its data in BR database.

### Flows

1- Fetch Zoho Branches of past 3 hours:

```javascript
https://books.zoho.in/api/v3/branches?organization_id={{$node["Helper"].json["zohoOrgId"]}}
```

2- Return single distributors using JS functions and add additional data into it.

```javascript
function formatDate(format, dateString = null){
    var date = new Date();
    if(dateString){
        date = new Date(dateString);
    }
    var mm = date.getMonth() + 1;
    var dd = date.getDate();
    var yyyy = date.getFullYear();
    var h= date.getHours();
    var i= date.getMinutes();
    var s= date.getSeconds();

    const map = {
        mm: mm > 9 ? mm : "0" + mm.toString(),
        dd: dd > 9 ? dd : "0" + dd.toString(),
        yyyy: yyyy,
        h: h > 9 ? h : "0" + h.toString(),
        i: i > 9 ? i : "0" + i.toString(),
        s: s > 9 ? s : "0" + s.toString(),
    }
    
    return format.replace(/mm|dd|yyyy|h|i|s/gi, matched => map[matched]);
}

var zohoBranches = $node["Zoho-GET-Branch"].json["branches"];
if(!zohoBranches){
    return [];
}
var returnData = [];
zohoBranches.forEach(branch => {

  if(1){
    returnData.push({
        json: {
            "distributor_erp_id": branch.branch_id,
            "name": branch.branch_name,
            "country": branch.address.country,
            "city": branch.address.city,
            "street": branch.address.street_address1+" "+branch.address.street_address2,
            "state": branch.address.state,
            "pincode": branch.address.postal_code,
            "email" :branch.email,
            "mobile":branch.phone,
            "is_available": (branch.is_branch_active === true) ? 1:0,
            "gstin_no":branch.tax_reg_no
        }
    });
  }
});
return returnData;

```


3- Create Distributors BR database:

```javascript
{{$node["Helper"].json.brBaseUrl}}/v1/distributor/create?key={{$node["Helper"].json.token}}&debug=1
```
  - In Body :
  ```javascript
  {{$node["SplitInBatches"].json}}
  ```


## Workflow

[On Click on this, you will be redirected to workflow](https://int.beatroute.io/workflow/20)

To call Zoho Apis we need to integrate Zoho's 0Auth token:

 1- OAuth Redirect URL:
   > https://int.beatroute.io/rest/oauth2-credential/callback
   
 2- Authorization URL:
   > https://accounts.zoho.in/oauth/v2/auth?

 3- Access Token URL:
   > https://accounts.zoho.in/oauth/v2/token?
   
 4- Client ID:
   > 1000.XNHV0LXKCZNQ92CV1WV6GADZVXJ4CC

 5- Client Secret 
   > f6ff767160b8a63aa8e16e85bf356ee5971270dd01
 
 6- Scope
   > ZohoBooks.fullaccess.all

 7- Auth URI Query Parameters
   > access_type=offline

![](./resources/zoho-customer->Zoho-Distributor->BR.png)
