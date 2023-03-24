# STSAFE-A110-AWS-ESP32-test

This communicates STMicro STSAFE-A110 secure chip from ESP32 and connects to AWS IOT configured for "Multi-Account-Registration"

for Multi-Account-Registration, check the [AWS website](https://pages.awscloud.com/iot-core-early-registration.html)  

# Requirements

Platformio(PLATFORM: Espressif 32 6.1.0) with VS Code environment.  
install "Espressif 32" platform definition on Platformio  

you need to run [stsafe-esp32-test](https://github.com/kmwebnet/stsafe_esp32_test.git) to extract device certificate.    
And register it to your AWS Account by using Multi-Account-Registration-enabled AWS CLI prior to use this code.       

# Environment reference
  
  Espressif ESP32-DevkitC  
  this project initialize I2C port   
  pin assined as below:  

      I2C SDA GPIO_NUM_21  
      I2C SCL GPIO_NUM_22  
       
  STMicro STSAFE-A110 SPL02   

# Usage  

"git clone --recursive " on your target directory. and you need to change a serial port number which actually connected to ESP32 in platformio.ini.    

Create policy by AWS CLI. Run this only once.      

aws iot create-policy --policy-name wildcardpolicy --policy-document file://wildcardpolicy"   
{
    "policyName": "wildcardpolicy",
    "policyArn": "arn:aws:iot:ap-northeast-1:XXXXXXXXXXXX:policy/wildcardpolicy",
    "policyDocument": "{\n    \"Version\": \"2012-10-17\",\n    \"Statement\": [\n        {\n            \"Effect\": \"Allow\",\n            \"Action\": [\n                \"iot:Connect\",\n                \"iot:Publish\",\n                \"iot:Receive\",\n                \"iot:Subscribe\"\n            ],\n            \"Resource\": [\n                \"*\"\n            ]\n        }\n    ]\n}\n",
    "policyVersionId": "1"
}

Register certificate by AWS CLI.Run for each device.    

```
aws iot register-certificate-without-ca --certificate-pem file://certs/0123XXXXXXXXXXXX01 --status ACTIVE    
{
    "certificateArn": "arn:aws:iot:ap-northeast-1:XXXXXXXXXXXX:cert/56366869fd...96b05161",
    "certificateId": "56366869fd8....096b05161"
}

aws iot attach-policy --target "arn:aws:iot:ap-northeast-1:XXXXXXXXXXXX:cert/56366869....96b05161" --policy-name wildcardpolicy
```

Get AWS IoT access endpoint

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
{
    "endpointAddress": "XXXXXXXXXX-ats.iot.ap-northeast-1.amazonaws.com"
}
```

The above authority and role settings are for testing purposes, so please check and change them yourself in the production environment.

replace definitions to your own by executing pio.exe run -t menuconfig  

Example Connection Configuration =>
WiFi SSID
WiFi Password

Example Configuration =>  
Endpoint of the MQTT broker to connect to "XXXXXXXXXXXX-ats.iot.ap-northeast-1.amazonaws.com"       

# Run this project

just execute "Upload" on Platformio.   
# License

If a license is stated in the source code, that license is applied, otherwise the MIT license is applied, see LICENSE.  