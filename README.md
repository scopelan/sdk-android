Authorize.Net Android SDK
=========================

The Android SDK is meant to offer an easy approach to collecting payments
on the Android mobile devices.  It's an extension to the Authorize.Net Java
SDK.  The Android SDK provides a fast and easy way for Android developers
to quickly integrate mobile payments without having to write the boiler plate
code themselves that is necessary to communicate with the Authorize.Net gateway.

The SDK is comprised of the following Authorize.Net APIs:
    
    * AIM - Advanced Integration Method
    * Transaction Details
    * CIM - Customer Information Manager
    * ARB - Automated Recurring Billing
 
The core of the SDK involves getting an instance of an AbstractButton 
(net.authorize.android.button.AbstractButton), linking that button click to 
an Intent that is created via net.authorize.android.AuthNet, adding the extra
data components, and then finally launching the Intent with an Intent result 
callback.

The callbacks offer the developer access to the result data.  It is up to the 
app developer to implement specific business logic to handle the results
and notify the end user of the results of the transaction that was processed.
The 3 callbacks offered are listed here:

1. resultTransactionCanceled - the transaction was canceled
2. resultTransactionFailed - the transaction failed
3. resultTransactionSucceeded - the transaction succeeded

Examples of implementation and integration are as follows.  The examples do
not cover the app building process, but offer the necessary pieces of 
information to help the developer add Authorize.Net payment gateway function-
ality to their app.

Every example will require that the necessary Authorize.Net SDK components 
exist in the project (either create a new project, or update a current project). 

Here are those steps:

1. Add the Authorize.Net Android SDK library to your project in your 
environment workspace.  If you would like to easily import the SDK project into Eclipse, 
run 'unzip eclipse_files.zip' from the command line in the anet_android_sdk 
directory.  Make sure the files get unzipped directly into the root SDK 
directory.

There is also a jar file (anet_android_sdk/lib/anet-java-sdk-android-X.X.X.jar)
that needs to be referenced in your project.   

The SDK is built on the Android 2.2+ (API Level 8) platform and 
offers the ability of linking external projects as libraries.  The application 
you develop from the SDK will need to be linked this way within your IDE/environment.  
For more information, please reference 
http://developer.android.com/guide/developing/projects/projects-eclipse.html

2.  If your AndroidManifest.xml file doesn't already allow access to the
Internet and your phone state, make sure to add the following snippetd in the 
manifest element.

  <uses-permission android:name="android.permission.INTERNET"/>
  <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>

3. Add the following activity declarations to the AndroidManifest.xml within the
application element of your app.  Every activity declaration is NOT necessary,
only the ones that are explicitly used in the app are required.  However,
net.authorize.android.security.MobileMerchantAuthActivity IS required, since 
it is how transactions are authorized for each merchant account on the gateway.

    <!-- ** Security Activities - REQUIRED ** -->
    <activity android:name="net.authorize.android.security.MobileMerchantAuthActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.security.LoginBaseActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.security.LogoutBaseActivity"
        android:configChanges="keyboardHidden|orientation" />

    <!-- AIM Activities -->
    <activity android:name="net.authorize.android.aim.AuthCaptureActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.aim.AuthOnlyActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.aim.CaptureOnlyActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.aim.VoidActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.aim.CreditActivity"
        android:configChanges="keyboardHidden|orientation" />
    
    <!-- Reporting Activities -->
    <activity android:name="net.authorize.android.reporting.SettledBatchListActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.reporting.BatchStatisticsActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.reporting.TransactionListActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.reporting.UnsettledTransactionListActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.reporting.TransactionDetailsActivity"
        android:configChanges="keyboardHidden|orientation" />

    <!-- CIM Activities -->
    <activity android:name="net.authorize.android.cim.CreateCustomerPaymentProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.CreateCustomerProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.CreateCustomerProfileTransactionActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.CreateCustomerShippingAddressActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.DeleteCustomerPaymentProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.DeleteCustomerProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.DeleteCustomerShippingAddressActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.GetCustomerPaymentProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.GetCustomerProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.GetCustomerProfileIdsActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.GetCustomerShippingAddressActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.UpdateCustomerPaymentProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.UpdateCustomerProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.UpdateCustomerShippingAddressActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.UpdateSplitTenderGroupActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.cim.ValidateCustomerPaymentProfileActivity"
        android:configChanges="keyboardHidden|orientation" />
    
    <!-- ARB Activities -->
    <activity android:name="net.authorize.android.arb.CancelSubscriptionActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.arb.CreateSubscriptionActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.arb.GetSubscriptionStatusActivity"
        android:configChanges="keyboardHidden|orientation" />
    <activity android:name="net.authorize.android.arb.UpdateSubscriptionActivity"
        android:configChanges="keyboardHidden|orientation" />

4.  The developer will need to create a layout that will be used to collect
the account login id and password.  These two pieces of information should be
EditText objects.  Additionally a cancel and validate/ok button will need to
be placed in the layout as well.  They don't have to be visible, but they must
exist.  The SDK takes the Android id's of these components and uses them when
it's necessary to login to the Authorize.Net gateway.  The logic is handled by
the SDK, but the UI is flexible and must be implemented by the developer.  Step
#5 below, in the test code, will cover that in more detail.
 

Test Code - Basic - Advanced Integration Method (AIM)
======================================================

The SDK offers many aspects of what Authorize.Net offers with their API gateway, 
but this example will cover the 
process of Authorization and Capture(AuthCapture).  This is by far the easiest and most 
common use case of the SDK. 

1.  In the project, create a new class called SampleActivity, and make the class
extend net.authorize.android.SimpleActivity.  SimpleActivity is a helper class 
that extends the core Activity class and handles the logistics of sub-Activities and 
callback control.

2.  Create a new layout called authnet_sample.xml and put the following code into
it.

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
      android:id="@+id/AuthNetSampleLayout"
      android:layout_height="match_parent"
      android:layout_width="match_parent" 
      android:gravity="fill_vertical"
      android:orientation="vertical" 
      android:paddingTop="10dp"/>
 
3.  Create another new layout called authnet_mobile_merchant_auth_dialog.xml and put
the following code into it.
 
    <RelativeLayout
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:orientation="horizontal"
      android:layout_width="fill_parent"
      android:layout_height="fill_parent"
      android:background="#ffffff">
    
      <TextView android:id="@+id/authnet_prompt_text" android:text="Enter your Authorize.Net login ID and password"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_centerHorizontal="true"
      android:textSize="18dip"
      android:textColor="#3a2014"
      android:gravity="center"/>
    
      <TextView android:id="@+id/authnet_loginid_text" android:text="Login ID"
      android:paddingTop="10dp"
      android:layout_centerHorizontal="true"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:textSize="16dp"
      android:textColor="#173b55"
      android:textStyle="bold"
      android:layout_below="@id/authnet_prompt_text"/>
    
      <EditText android:id="@+id/authnet_loginid_edit"
      android:textSize="16dp"
      android:textColor="#3a2014"
      android:layout_height="wrap_content"
      android:layout_width="250px"
      android:layout_centerHorizontal="true"
      android:layout_below="@id/authnet_loginid_text"
      android:singleLine="true" />
    
      <TextView android:id="@+id/authnet_password_text" android:text="Password"
      android:paddingTop="10dp"
      android:layout_centerHorizontal="true"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:textSize="16dp"
      android:textColor="#173b55"
      android:textStyle="bold"
      android:layout_below="@id/authnet_loginid_edit"/>
    
      <EditText android:id="@+id/authnet_password_edit"
      android:password="true"
      android:textSize="16dp"
      android:textColor="#3a2014"
      android:layout_height="wrap_content"
      android:layout_width="250px"
      android:layout_centerHorizontal="true"
      android:layout_below="@id/authnet_password_text"
      android:singleLine="true" />
    
      <TextView android:id="@+id/authnet_error_text" android:text=""
      android:layout_centerHorizontal="true"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:textSize="16dp"
      android:textColor="#e50023"
      android:textStyle="bold"
      android:layout_below="@id/authnet_password_edit"/>
    
      <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/authnet_auth_buttons"
        android:layout_below="@id/authnet_error_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp">
    
        <Button android:id="@+id/authnet_auth_cancel_button"
        android:text="Cancel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/authnet_error_text"/>
    
        <Button android:id="@+id/authnet_auth_login_button"
        android:text="Login"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/authnet_error_text"/>
      </LinearLayout>
    
    </RelativeLayout>
   
4.  Add the following imports to the SampleActivity class.

    import java.math.BigDecimal;
    import java.util.Calendar;
    import java.util.HashMap;
    
    import net.authorize.android.SimpleActivity;
    import net.authorize.android.AuthNet;
    import net.authorize.android.AuthNetActivityBase;
    import net.authorize.Environment;
    import net.authorize.ResponseReasonCode;
    import net.authorize.TransactionType;
    import net.authorize.android.button.AuthNetButton;
    import net.authorize.data.Address;
    import net.authorize.data.Customer;
    import net.authorize.data.EmailReceipt;
    import net.authorize.data.Order;
    import net.authorize.data.ShippingCharges;
    import net.authorize.data.creditcard.CreditCard;
    import net.authorize.xml.MessageType;
    import net.authorize.xml.Result;
    import android.app.AlertDialog;
    import android.content.DialogInterface;
    import android.content.Intent;
    import android.os.Bundle;
    import android.view.View;
    import android.view.View.OnClickListener;
    import android.widget.LinearLayout;

5.  Override the onCreate method with the following code.  The authNetObj is 
    getting initialized for the sandbox environment and a button is created that when
    clicked, will launch an Intent to execute an authorization and capture transaction.

    /**
     * Called when the activity is first created.
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // load merchant info
        setContentView(R.layout.authnet_sample);

        authNetObj = AuthNet.getInstance(Environment.SANDBOX, 
                R.layout.authnet_mobile_merchant_auth_dialog, 
                R.id.authnet_loginid_edit,
                R.id.authnet_password_edit,
                R.id.authnet_auth_cancel_button,
                R.id.authnet_auth_login_button);
        
        // prepare the authCapture button
        AuthNetButton authCaptureButton = authNetObj.getButton(this,
                "Authorize and Capture");
        authCaptureButton.setOnClickListener(new OnClickListener() {
            
            public void onClick(View arg0) {
                launchAuthCaptureIntent();
            }
        });

        LinearLayout layout = (LinearLayout)findViewById(R.id.AuthNetSampleLayout);
        layout.addView(authCaptureButton);
    }

6. Implement the launchAuthCaptureIntent() method.  This method is handling
   the creation of the Intent to perform an AuthCapture.  Since 
   this is a simple example, the Intent is only capturing a credit card and some basic
   order information.  In addition to sending off the activity to be executed, a callback 
   is being passed in to perform post-transaction processing on the result data.

    /**
     * Activity launcher for authCapture transactions.
     */
    private void launchAuthCaptureIntent() {
        String refId = Long.toString(System.currentTimeMillis());
        BigDecimal totalAmount = new BigDecimal(19.99);

        /*
         * listing these out only to show the required 'parameter set'
         * that should be passed into the create Intent method.
         */
        CreditCard creditCard = CreditCard.createCreditCard();
        creditCard.setCreditCardNumber("4111111111111111");
        Calendar expCal = Calendar.getInstance();
        expCal.add(Calendar.YEAR, 4);
        creditCard.setExpirationDate(expCal.getTime());
        
        Order order = Order.createOrder();
        order.setTotalAmount(new BigDecimal(19.95));
        order.setDescription("Android Test Order");
        
        Customer customer = null;
        Address shippingAddress = null;
        ShippingCharges shippingCharges = null;
        EmailReceipt emailReceipt = null;
        HashMap<String, String> merchantDefinedFields = new HashMap<String, String>();
        merchantDefinedFields.put("notes", "sent from SampleActivity");
        
        Intent authNetIntent = authNetObj.createAIMAuthCaptureIntent(this, refId,
                totalAmount, creditCard, order, customer, shippingAddress,
                shippingCharges, emailReceipt, merchantDefinedFields);
        /*
         * final launch command to spawn the Activity and provide a callback
         * where the result data can be processed
         */
        authNetIntent.putExtra(AuthNetActivityBase.EXTRA_DUPLICATE_TXN_WINDOW_SECS, 5);
        launchSubActivity(authNetIntent, createPaymentIntentResultCallback());
    }

7. Implement the callback mechanism.  The three methods described above for 
   the callback are implemented here.  For this example, a simple Alert dialog
   will be displayed that shows what happened with the Activity and sub-
   sequent AuthCapture transaction.

    /**
     * Creates a payment Intent ResultCallback.
     *
     * @return ResultCallback
     */
    private ResultCallback createPaymentIntentResultCallback() {
        return new SimpleActivity.ResultCallback() {

            public void resultTransactionCanceled(
                    Enum<?> txnType) {
                showAlert((TransactionType)txnType + " canceled","");
            }

            public void resultTransactionFailed(
                    Enum<?> txnType, Result result) {

                MessageType msgType = MessageType.E00000;
                StringBuilder errorBuilder = 
                    new StringBuilder(msgType.getValue()).append(": ")
                    .append(msgType.getText());

                net.authorize.aim.Result aimResult = 
                    (net.authorize.aim.Result)result;
                
                errorBuilder = new StringBuilder();
                if(aimResult.getTransactionResponseErrors().size() > 0) {
                    for(ResponseReasonCode respReasonCode : aimResult.getTransactionResponseErrors()) {
                        errorBuilder.append(respReasonCode.getResponseReasonCode()).append(": ")
                        .append(respReasonCode.getReasonText()).append("\n");
                    }
                } else if(aimResult.getMessages().size() > 0) {
                    for(MessageType msgType1 : aimResult.getMessages()) {
                        errorBuilder.append(msgType1.getValue()).append(": ")
                        .append(msgType1.getText()).append("\n");
                    }
                } 
                showAlert((TransactionType)txnType + " failed", errorBuilder.toString());
            }

            public void resultTransactionSucceeded(
                    Enum<?> txnType, Result result) {

                net.authorize.aim.Result aimResult = (net.authorize.aim.Result)result;
                showAlert((TransactionType)txnType + " success", 
                        "authCode: " + aimResult.getAuthCode() + "\n" +
                        "refTransId: " + aimResult.getTransId());
            }
        };
    }

8. Add a little helper method that will pop up an alert dialog to the user
whenever a the sub-Activity is finished.

    /**
     * Create a simple alert dialog.
     * 
     * @param title
     * @param message
     */
    private void showAlert(String title, String message) {
        // Show an error to the user
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setMessage(message).setTitle(title).setPositiveButton("OK",
                new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int id) {
                    }
                });
        AlertDialog alert = builder.create();
        alert.show();
    }

9.  Run your sample app and when you click on the "Authorize and Capture" button, you
should be prompted with a dialog that asks you to enter in your Login Id and Password.
These are your Authorize.Net account login credentials.  If this is the first time you are 
performing this login from a new device (either real or virtual), the login will fail and state
that the device has been registered but is pending approval.  You will need to login to the 
sandbox and enable the new device.  You can find this by going to :

Home > Account (Settings) > Security Settings (Mobile Device Management)

Once there, you should see your new device in a 'Pending' state.  Click on it and enable 
it.  Click on the Login button again, and the transaction should succeed and you will
be prompted with an authCode and refTransId. 



