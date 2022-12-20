

# DelegateCall or Storage Collision Attack on Smart Contracts

It allows you to call the code of the callee contract from the caller with the storage context of the caller.

![image](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fbd68e45d-6238-483a-bf8f-393db30ac39c_2650x1572.png)

![image](https://user-images.githubusercontent.com/82324643/208247768-59eef9e3-1e2e-4a47-a2c4-76b14b8680b2.png)




#### when change the order of storage variable of `Contract B`


![image](https://user-images.githubusercontent.com/82324643/208247777-c89fb1e5-2597-45ae-a0d7-a2682b885bff.png)

When we run `DELEGATECALL` the `“num”` value is going to be stored in `storage slot 2` for `Contract A` which maps to the `“value”` state variable. The same applies for when `“value”` is stored its going to update `slot 0` which maps to the `“num”` state variable.


## Exploit with delegatecall



![Screenshot (101)](https://user-images.githubusercontent.com/82324643/208251425-3010c0a1-c02d-4085-bfa9-4e972a0b6d57.png)


