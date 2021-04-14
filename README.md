# PayPal Payouts Java SDK v2

![Home Image](homepage.jpg)

__Welcome to PayPal Java SDK.__ This repository contains PayPal's Java SDK for Payouts and samples for [v1/payments/payouts](https://developer.paypal.com/docs/api/payments.payouts-batch/v1/) APIs.

This is a part of the next major PayPal SDK. It includes a simplified interface to only provide simple model objects and blueprints for HTTP calls. This repo currently contains functionality for PayPal Payouts APIs which includes [Payouts](https://developer.paypal.com/docs/api/payments.payouts-batch/v1/).

Please refer to the [PayPal Payouts Integration Guide](https://developer.paypal.com/docs/payouts/) for more information. Also refer to [Setup your SDK](https://developer.paypal.com/docs/payouts/reference/setup-sdk/) for additional information about setting up the SDK's. 

## Prerequisites

Java JDK 8 or higher

An environment which supports TLS 1.2 (see the TLS-update site for more information)

## Usage
### Binaries

It is not necessary to fork this repository for using the PayPal SDK. Please take a look at [PayPal Payouts SDK](https://developer.paypal.com/docs/payouts/reference/setup-sdk/#install-the-sdk) for configuring and working with SDK without forking this code.

For contributing to this repository or using the samples you can fork this repository.

### Setting up credentials

Get client ID and client secret by going to https://developer.paypal.com/developer/applications and generating a REST API app. Get <b>Client ID</b> and <b>Secret</b> from there.

```java
package com.paypal;
import com.paypal.core.PayPalEnvironment;
import com.paypal.core.PayPalHttpClient;
public class Credentials {
    static String clientId = "CLIENT-ID";
    static String secret = "CLIENT-SECRET";
    
    // Creating a sandbox environment
    private static PayPalEnvironment environment = new PayPalEnvironment.Sandbox(clientId, secret);
    
    // Creating a client for the environment
    static PayPalHttpClient client = new PayPalHttpClient(environment);
}
```

## Examples
### Creating a Payouts
This will create a Payout and print batch_id for the created Payout.

```java
package com.paypal;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import com.paypal.http.HttpResponse;
import com.paypal.http.exceptions.HttpException;
import com.paypal.payouts.*;

public class CreatePayoutExample {
	public static void main(String[] args) {

		// Construct a request object and set desired parameters
		// Here, CreatePayoutRequest() creates a POST request to /v1/payments/payouts
		List<PayoutItem> items = IntStream
                .range(1, 6)
                .mapToObj(index -> new PayoutItem()
                        .senderItemId("Test_txn_" + index)
                        .note("Your 5$ Payout!")
                        .receiver("payout-sdk-" + index + "@paypal.com")
                        .amount(new Currency()
                                .currency("USD")
                                .value("1.00")))
                .collect(Collectors.toList());

        CreatePayoutsRequest request = CreatePayoutRequest()
                .senderBatchHeader(new SenderBatchHeader()
                        .senderBatchId("Test_sdk_" + RandomStringUtils.randomAlphanumeric(7))
                        .emailMessage("SDK payouts test txn")
                        .emailSubject("This is a test transaction from SDK")
                        .note("Enjoy your Payout!!")
                        .recipientType("EMAIL"))
                .items(items);

		try {
			// Call API with your client and get a response for your call
			HttpResponse<CreatePayoutResponse> response = Credentials.client.execute(request);

			// If call returns body in response, you can get the de-serialized version by
			// calling result() on the response
			CreatePayoutResponse payouts = response.result();
			System.out.println("Payouts Batch ID: " + payouts.batchHeader().payoutBatchId());
			payouts.links().forEach(link -> System.out.println(link.rel() + " => " + link.method() + ":" + link.href()));
		} catch (IOException ioe) {
			if (ioe instanceof HttpException) {
				// Something went wrong server-side
				HttpException he = (HttpException) ioe;
				System.out.println(he.getMessage());
				he.headers().forEach(x -> System.out.println(x + " :" + he.headers().header(x)));
			} else {
				// Something went wrong client-side
			}
		}
	}
}
```

### Retrieve a Payouts Batch
This will retrieve a payouts batch
```java
package com.paypal;

import java.io.IOException;

import com.paypal.http.HttpResponse;
import com.paypal.http.exceptions.HttpException;
import com.paypal.payouts.*;

public class GetPayoutsExample {
	public static void main(String[] args) {
        //Retrieve payouts batch details, a maximum of 1000 items can be fetched in a single call. 
        //Follow pagination to get all the items(Payouts supports a maximum of 15,000 items in a batch.
		PayoutsGetRequest request = new PayoutsGetRequest(batchId)
                        .page(1)
                        .pageSize(10)
                        .totalRequired(true);

		try {
			// Call API with your client and get a response for your call
			HttpResponse<PayoutBatch> response = Credentials.client.execute(request);

			// If call returns body in response, you can get the de-serialized version by
			// calling result() on the response
			PayoutBatch payout = response.result();
			System.out.println("Payouts Batch ID: " + payouts.batchHeader().payoutBatchId());
            System.out.println("Payouts Batch Status: " + payouts.batchHeader().batchStatus());
			payout.links()
					.forEach(link -> System.out.println(link.rel() + " => " + link.method() + ":" + link.href()));
		} catch (IOException ioe) {
			if (ioe instanceof HttpException) {
				// Something went wrong server-side
				HttpException he = (HttpException) ioe;
				System.out.println(he.getMessage());
				he.headers().forEach(x -> System.out.println(x + " :" + he.headers().header(x)));
			
    public class MainActivity extends AppCompatActivity {
TextView whichCard;
ImageButton payNow,cancel;
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    Toast.makeText(this, ""+ Constants.whichCard, Toast.LENGTH_SHORT).show();
    try {
        if (!Constants.whichCard.equals("")) {
            whichCard.setText(Constants.whichCard);
        }
        else {
            Toast.makeText(this, "Nullll", Toast.LENGTH_SHORT).show();
        }
    }catch (Exception e){}

    payNow= findViewById(R.id.payNow);
    cancel= findViewById(R.id.cancelPay);


    cancel.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {

        }
    });
    payNow.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent newPayIntent = new Intent(MainActivity.this, PayActivity.class);

            newPayIntent.putExtra("merchantId", "459");
            newPayIntent.putExtra("txnscamt", "0"); //Fixed. Must be �0�
            newPayIntent.putExtra("loginid", "459");
            newPayIntent.putExtra("password", "Test@123");
            newPayIntent.putExtra("prodid", "NSE");
            newPayIntent.putExtra("txncurr", "INR"); //Fixed. Must be �INR�
            newPayIntent.putExtra("clientcode", "001");
            newPayIntent.putExtra("custacc", "100000036900");
            newPayIntent.putExtra("amt", "50.000");//Should be 3 decimal number i.e 1.000
            newPayIntent.putExtra("txnid", "01");
            newPayIntent.putExtra("date", "25/08/2015 18:31:00");//Should be in same format
            newPayIntent.putExtra("bankid", "2019"); //Should be valid bank id

            //use below Production url only with Production "Library-MobilePaymentSDK", Located inside PROD folder
            //newPayIntent.putExtra("ru","https://payment.atomtech.in/mobilesdk/param"); //ru FOR Production

            //use below UAT url only with UAT "Library-MobilePaymentSDK", Located inside UAT folder
            newPayIntent.putExtra("ru", "https://paynetzuat.atomtech.in/mobilesdk/param"); // FOR UAT (Testing)

            //Optinal Parameters
            newPayIntent.putExtra("customerName", "Sweta sap"); //Only for Name
            newPayIntent.putExtra("customerEmailID", "sap.sweta@atomtech.in");//Only for Email ID
            newPayIntent.putExtra("customerMobileNo", "9876043210");//Only for Mobile Number
            newPayIntent.putExtra("billingAddress", "Mumbai");//Only for Address
            newPayIntent.putExtra("optionalUdf9", "OPTIONAL DATA 1");// Can pass any data

            startActivityForResult(newPayIntent, 1);
        }
    });

}

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == 1)
    {
        if (data != null)
        {
            String message = data.getStringExtra("status");
            String[] resKey = data.getStringArrayExtra("responseKeyArray");
            String[] resValue = data.getStringArrayExtra("responseValueArray");

            if(resKey!=null && resValue!=null)
            {
                for(int i=0; i<resKey.length; i++)
                    System.out.println("  "+i+" resKey : "+resKey[i]+" resValue : "+resValue[i]);
            }
            Toast.makeText(this, message, Toast.LENGTH_LONG).show();

        }
    }

}
}

Kindly guide me how to solve this issue.

Share Improve this question Follow
asked
Nov 5 '19 at 12:31

00Curly
1●22 bronze badges edited
Nov 5 '19 at 12:49

Check the dependancy you are using, there is issue in com.atom.mobilepaymentsdk.PayActivity means in the library I guess. Check if you are using latest version or not. – Prashant Sable Nov 5 '19 at 13:00
no,its not latest version , actual i'm using android-Version 28 and the library has android-Version 24. – 00Curly Nov 5 '19 at 13:09 
} else {
				// Something went wrong client-side
			}
		}
	}
}
```
## Running tests

To run integration tests using your client id and secret, run the `test` gradle command with the `-Pintegration` flag
```sh
$ PAYPAL_CLIENT_ID=your_client_id PAYPAL_CLIENT_SECRET=your_client_secret ./gradlew clean test -Pintegration
```

You may use the client id and secret above for demonstration purposes.


## Samples

You can start off by trying out [Payouts samples](payouts-sdk-sample/src/main/java/com/paypal/RunAll.java).

To try out different samples head to [this link](payouts-sdk-sample/src/main/java/com/paypal).

Note: Update the `PayPalClient.java` with your sandbox client credentials or pass your client credentials as environment variable while executing the samples.

## License
Code released under [SDK LICENSE](LICENSE)
