1.launch an ec2

2.modify the security groups----
Security groups inbound rules ----
3000 — anywhere(by default react js will run on port 3000 )


3.Connect ec2 with ssh client

4.install the following dependencies
------------------------------------
1.node js – yum install nodejs -y

2.react js – npx create-react-app <folder name>

3.cd <folder name>

4.npm — npm install

5.npm start 

>>>if u have ur own code then no need to run npx command.

>>>npx command will create new react app in the mentioned folder

>>>along with the app it will create all dependency files like package.json,package-lock-json,src…etc (in the src folder only our app runs)

>>>the app will start with npm start command

>>>now u can able to access react js page in the browser with the publicIp:3000


