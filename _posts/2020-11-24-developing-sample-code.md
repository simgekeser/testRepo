---
title: Implementing the demo
description: 15
---

<p>HUAWEI Identity kit API is used to display the address selection page when your app needs to use the user’s address-related information, such as name, contact, and detailed address.</p>
<p><strong>1.To get the address information, the first thing is to create a new UserAddressRequest and call the getUserAddress API.</strong></p>
<pre><div id="copy-button10" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> public void getUserAddress() {
        UserAddressRequest req = new UserAddressRequest();
        Task<GetUserAddressResult> task = Address.getAddressClient(activity).getUserAddress(req);
        task.addOnSuccessListener(new OnSuccessListener<GetUserAddressResult>() {
            @Override
            public void onSuccess(GetUserAddressResult result) {
                Log.i(TAG, "onSuccess result code:" + result.getReturnCode());
                try {
                    startActivityForResult(result);
                } catch (IntentSender.SendIntentException e) {
                    e.printStackTrace();
                }
            }
        }).addOnFailureListener(new OnFailureListener() {
            @Override
            public void onFailure(Exception e) {
                Log.i(TAG, "on Failed result code:" + e.getMessage());
            }
        });
    } </code>
<p>If result from getuseraddress api is successful, call the startActivityForResult method in onSuccess.Otherwise, an error message is displayed in onFailure.</p></pre>
<p><strong>2. Call this function by pressing get address information button in main activity.</strong></p>
<pre><div id="copy-button11" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> getAddress.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                identityKitHelper.getUserAddress();
            }});</code></pre>
            
<p><strong>3. Display address management page.</strong></p>
<pre><div id="copy-button12" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> private void startActivityForResult(GetUserAddressResult result) throws IntentSender.SendIntentException {
        Status status = result.getStatus();
        if (result.getReturnCode() == 0 && status.hasResolution()) {
            Log.i(TAG, "the result had resolution.");
            status.startResolutionForResult(activity, GET_ADDRESS);
        } else {
            Log.i(TAG, "the response is wrong, the return code is " + result.getReturnCode());
        }
    }</code></pre>
    
<p>The address selection page will be displayed by calling the startResolutionForResult method of the status. After making the changes regarding the address, the user can complete the address selection process by clicking the OK button at the bottom of the page.</p>
<p><strong>4. Handle the Response </strong></p>
<p>If the result code is equal to Activity.RESULT_OK in onActivityResult, the address information will be accessed.</p>
<pre><div id="copy-button13" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
            Log.i("TAG", "requestCode=" + requestCode + ", resultCode=" + resultCode);
            if (resultCode == Activity.RESULT_OK) {
                UserAddress userAddress = UserAddress.parseIntent(data);
                userAddressModel.setName(userAddress.getName());
                userAddressModel.setPhoneNumber(userAddress.getPhoneNumber());
                userAddressModel.setEmail(userAddress.getEmailAddress());
                userAddressModel.setAddress(userAddress.getAddressLine2());
                userAddressModel.setProvince(userAddress.getAdministrativeArea());
                userAddressModel.setCity(userAddress.getLocality());
                userAddressModel.setCountry(userAddress.getCountryCode());
                userAddressModel.setPostalCode(userAddress.getPostalNumber());
                resultTv.setText("Name : " + userAddressModel.getName()+ '\n'+
                        "Email : " + userAddressModel.getEmail()+'\n'+
                        "Phone Number : " +userAddressModel.getPhoneNumber()+'\n'+
                        "Address : " +userAddressModel.getAddress()+'\n'+
                        "Province : " +userAddressModel.getProvince()+'\n'+
                        "City : " + userAddressModel.getCity()+'\n'+
                        "County : " +userAddressModel.getCountry()+'\n'+
                        "PostalCode : " +userAddressModel.getPostalCode());
            } else {
                Log.d("TAG", "onActivityResult: error");
            }
    } </code></pre>
    
<p><strong>5. Full code of Identity kit Helper Class</strong></p>
<pre><div id="copy-button12" class="copy-btn" title="Copy" onclick="copyCode(this.id)"></div><code> public class IdentityKitHelper{
    private String TAG = "identitykitsuperdemo";
    private int GET_ADDRESS = 1000;
    private Activity activity;
    public IdentityKitHelper(Activity activity) {
        this.activity = activity;
    }
    public void getUserAddress() {
        UserAddressRequest req = new UserAddressRequest();
        Task<GetUserAddressResult> task = Address.getAddressClient(activity).getUserAddress(req);
        task.addOnSuccessListener(new OnSuccessListener<GetUserAddressResult>() {
            @Override
            public void onSuccess(GetUserAddressResult result) {
                Log.i(TAG, "onSuccess result code:" + result.getReturnCode());
                try {
                    startActivityForResult(result);
                } catch (IntentSender.SendIntentException e) {
                    e.printStackTrace();
                }
            }
        }).addOnFailureListener(new OnFailureListener() {
            @Override
            public void onFailure(Exception e) {
                Log.i(TAG, "on Failed result code:" + e.getMessage());
            }
        });
    }
    private void startActivityForResult(GetUserAddressResult result) throws IntentSender.SendIntentException {
        Status status = result.getStatus();
        if (result.getReturnCode() == 0 && status.hasResolution()) {
            Log.i(TAG, "the result had resolution.");
            status.startResolutionForResult(activity, GET_ADDRESS);
        } else {
            Log.i(TAG, "the response is wrong, the return code is " + result.getReturnCode());
        }
    }
}
