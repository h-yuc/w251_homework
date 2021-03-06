
### Face_Detector TX2
   	docker build -t detector -f DockerFile_fd .
   	docker run --name detector --network hw3 -e DISPLAY=$DISPLAY --privileged -v /tmp:/tmp --rm -ti detector

  Save faces.png to broker. Topic: hw3 and QoS = 2
    
### MQTT_broker TX2
	docker build -t mosquitto -f DockerFile_broker .
   	docker run --name mosquitto --network hw3 -p 1883:1883 --rm -ti mosquitto
	
### Forwarder TX2
	docker build -t mosquitto -f DockerFile_broker .
   	docker run --name mosquitto --network hw33 -p 1883:1883 --rm -ti mosquitto    
   
   Subscribe messages for `hw3` from detector with QoS = 2, then publish messages to MQTTbroker in the cloud. Topic: facedetection and QoS = 2.
    
### MQTT_broker vsi
	docker build -t mosquito -f DockerFile_broker .
	docker run --name mosquito --network hw3 -p 1883:1883 --rm -ti mosquito

### Image processing on TX2

	docker build -t saver -f DockerFile_saver .
	docker run --name saver -v /mnt/w251bucket-hw3/:/mnt/w251bucket-hw3/ --network hw3 --rm -ti saver
 
  Subscribe messages for `facedetection` with QoS = 2. Save faces in object storage bucket to `w251bucket-hw3`.
   
### Object storage

	sudo s3fs w251bucket-hw3 /mnt/w251bucket-hw3 -o nonempty -o passwd_file=$HOME/.cos_creds -o sigv2 -o use_path_request_style -o url=http://s3.us-east.cloud-object-storage.appdomain.cloud
	
  image:
  
	http://s3.us-east.cloud-object-storage.appdomain.cloud/w251bucket-hw3/img1015.png
	http://s3.us-east.cloud-object-storage.appdomain.cloud/w251bucket-hw3/img1031.png
	http://s3.us-east.cloud-object-storage.appdomain.cloud/w251bucket-hw3/img1067.png
	http://s3.us-east.cloud-object-storage.appdomain.cloud/w251bucket-hw3/img1086.png


![image](img1015.png)
![image](img1031.png)
![image](img1067.png)
![image](img1086.png)

