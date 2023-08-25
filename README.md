# UddoktaPay SDK Integration for Android

This document provides instructions for integrating the UddoktaPay Android SDK into your project. The current SDK version is ultra.2.0.

## Getting Started
Follow these steps to integrate the SDK into your project:

1. Add this dependency to your `build.gradle`:

```gradle
// UddoktaPay SDK
implementation 'com.github.uddoktapay:UddoktaPayAndroidSDK:ultra.2.0'
```

2. Add the `okhttp3` dependency to your `build.gradle`:

```gradle
implementation 'com.squareup.okhttp3:okhttp:5.0.0-alpha.2'
```

3. Add the JitPack repository to your root `build.gradle`:

In your `settings.gradle`, add the following at the end of the repositories:

```gradle
maven { url 'https://jitpack.io' }
```

## Constants for Payment
Add the following code inside your `AppCompatActivity`:

```java
// Constants for payment
private static final String FULL_NAME = "John Doe";
private static final String EMAIL = "test@gmail.com";
private static final String API_KEY = "982d381360a69d419689740d9f2e26ce36fb7a50";    
private static final String CHECKOUT_URL = "https://sandbox.uddoktapay.com/api/checkout-v2";
private static final String VERIFY_PAYMENT_URL = "https://sandbox.uddoktapay.com/api/verify-payment";
private static final String REDIRECT_URL = "https://uddoktapay.com";
private static final String CANCEL_URL = "https://uddoktapay.com";     
```

## Instance Variables
Add the following code inside your `AppCompatActivity`:

```java
// Instance variables to store payment information
private String storedFullName;
private String storedEmail;
private String storedAmount;
private String storedInvoiceId;
private String storedPaymentMethod;
private String storedSenderNumber;
private String storedTransactionId;
private String storedDate;
private String storedFee;
private String storedChargedAmount;

private String storedMetaKey1;
private String storedMetaValue1;

private String storedMetaKey2;
private String storedMetaValue2;

private String storedMetaKey3;
private String storedMetaValue3;
```

## Payment Process
Note: Use this code when the payment button is clicked:

```java
// Set your metadata values in the map
Map<String, String> metadataMap = new HashMap<>();
metadataMap.put("CustomMetaData1", "Meta Value 1");
metadataMap.put("CustomMetaData2", "Meta Value 2");
metadataMap.put("CustomMetaData3", "Meta Value 3");

UddoktaPay.PaymentCallback paymentCallback = new UddoktaPay.PaymentCallback() {
    @Override
    public void onPaymentStatus(String status, String fullName, String email, String amount, String invoiceId,
        String paymentMethod, String senderNumber, String transactionId,
        String date, Map<String, String> metadataValues, String fee,String chargeAmount) {
        // Callback method triggered when the payment status is received from the payment gateway.
        // It provides information about the payment transaction.
        storedFullName = fullName;
        storedEmail = email;
        storedAmount = amount;
        storedInvoiceId = invoiceId;
        storedPaymentMethod = paymentMethod;
        storedSenderNumber = senderNumber;
        storedTransactionId = transactionId;
        storedDate = date;
        storedFee = fee;
        storedChargedAmount = chargeAmount;

        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                // Clear previous metadata values to avoid duplication
                storedMetaKey1 = null;
                storedMetaValue1 = null;
                storedMetaKey2 = null;
                storedMetaValue2 = null;
                storedMetaKey3 = null;
                storedMetaValue3 = null;

                // Iterate through the metadata map and store the key-value pairs
                for (Map.Entry<String, String> entry : metadataValues.entrySet()) {
                    String metadataKey = entry.getKey();
                    String metadataValue = entry.getValue();

                    if ("CustomMetaData1".equals(metadataKey)) {
                        storedMetaKey1 = metadataKey;
                        storedMetaValue1 = metadataValue;
                    } else if ("CustomMetaData2".equals(metadataKey)) {
                        storedMetaKey2 = metadataKey;
                        storedMetaValue2 = metadataValue;
                    } else if ("CustomMetaData3".equals(metadataKey)) {
                        storedMetaKey3 = metadataKey;
                        storedMetaValue3 = metadataValue;
                    }
                }

                // Update UI based on payment status
                if ("COMPLETED".equals(status)) {
                    // Handle payment completed case
                } else if ("PENDING".equals(status)) {
                    // Handle payment pending case
                } else if ("ERROR".equals(status)) {
                    // Handle payment error case
                }
            }
        });
    }
};

UddoktaPay uddoktapay = new UddoktaPay(UddoktaPayWebView, paymentCallback);
uddoktapay.loadPaymentForm(API_KEY, FULL_NAME, EMAIL, enteredAmount, CHECKOUT_URL, VERIFY_PAYMENT_URL, REDIRECT_URL, CANCEL_URL, metadataMap);
```

Feel free to replace `AppCompatActivity` with the appropriate class if necessary. Also, make sure you have the required imports for the classes and libraries used in the code.


## Authors
- [Ali Ajgor](https://github.com/help5g)
