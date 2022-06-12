# JAGA-API
The API was used to deploy ML model to Google Cloud. We created it using Flask in VScode. Here is the step for it 

#Install the environment
```
pip install virtualenv
virtualenv <PUT YOUR ENVIRONMENT NAME >
<YOUR ENVIRONMENT NAME>\Scripts\activate.bat
python -m pip install --upgrade pip
```

#Install the variable
```
pip install flask
pip install numpy 
pip install pandas 
pip install lightgbm
```

#input models (download the models, put in the same file as your environment) 
#The file type for the application we are using is .pkl and the machine learning mode is LightGBM

#download the app.py from the repository (also put in the same file as your environment) 

#run the application 
```
python -m flask run
```
-----------------------------------------------------------------------------------------------------------------------------------
##DEPLOY FLASK APPLICATION IN GOOGLE CLOUD 

#Create your Virtual Machine 


-----------------------------------------------------------------------------------------------------------------------------------
##CONNECTING SPREADSHEET DATA TO FIREBASE 

#Create your own Spreadsheet database 
![image](https://user-images.githubusercontent.com/99376250/173258025-ee6bf1bd-83ad-4079-980c-8143f7dc99c8.png)

#Hover to Extensions -> App Script 
![image](https://user-images.githubusercontent.com/99376250/173258064-9db7f4c9-1ae7-4cec-8e8f-69a9f244b79d.png)
![image](https://user-images.githubusercontent.com/99376250/173258081-52e93ea6-60f3-4571-92f4-5d9491326f93.png)

#Input your Firebase Library by copy paste this API KEY 
```
FIREBASE_API_KEY = 1VUSl4b1r1eoNcRWotZM3e87ygkxvXltOgyDZhixqncz9lQ3MjfT1iKFw
```
#Install the Latest Version 
![image](https://user-images.githubusercontent.com/99376250/173258200-94a5c0fa-4db9-41b2-b506-a92dc33d92c3.png)

#Generate your own Private Key in Google Cloud Console (API&Services -> Credentials -> Service Accounts). 
#Make sure the Google Cloud Platform that you are using is the same platform that you use for Cloud Firebase
#Here is some of the important things that you need to look at 
```
CLIENT_EMAIL
PRIVATE_KEY
PROJECT_ID
```
#Implemeting the Code. Next put your credentials first, in the top of your code. 
```
function datajaga() {
const email = "PUT THE YOUR EMAIL FROM CREDENTIALS->SERVICE ACCCOUNTS -> EMAIL that was chosen for App Engine";
   const key = "PUT YOUR API KEY ";
   const projectId = "YOUR PROJECT ID ";
   var firestore = FirestoreApp.getFirestore (email, key, projectId);
 .....
 }
```
#After putting your credentials, you could generate variabel that will be used for your function and where and how you will get the data 
```
{
...
// Mendapatkan data dari spreadsheet 
   var excel = SpreadsheetApp.getActiveSpreadsheet();
   var nama = "THE NAME OF YOUR SHEET"; 
   var data = excel.getSheetByName(nama); 

   //dapetin data
   var datacolumn = data.getLastColumn(); // kolom terakhir
   var datarow = data.getLastRow(); // row terakhir
   var datapertama = 2; // THE FIRST ROW THAT HAD THE DATA
   var rangedata = data.getRange(2,1,datarow-datapertama+1,datacolumn); // CREATING 5 COLUMN FOR THE PLACE

   // dapetin data 
   var sumberdata = rangedata.getValues();
   var panjangsumber = sumberdata.length; #ini bakal dipakai untuk looping 
...
}
```
#Looping To get the Data and the post it as a form of RealTime Database
```
 for (var i=0;i<panjangsumber;i++){

     if(sumberdata[i][1] !== '') {
       var data = {};
       var tanggal = sumberdata[i][0].toString(); //mengubah kolom tanggal dari number jadi tipe string
       var tambahtanggal = new Date(tanggal);
       var stringdata = JSON.stringify(tambahtanggal);
       var updatetanggal = stringdata.slice(1,11);
       
        //naruh data di tempatnya sesuai dengan baris (bukan kolom)
       data.date = updatetanggal;
       data.days = sourceData[i][1];
       data.category = sourceData[i][2];
       data.adress = sourceData[i][3];
       data.x = sourceData[i][4];
       data.y = sourceData[i][5];
    
       firestore.createDocument("<NAMA REALTIMEDATABASE>",data);
     }
    
  }
  ...
  }
```

#The output will be in form of JSON file in the RealTimeDatabase
![image](https://user-images.githubusercontent.com/99376250/173258782-864e5205-007e-4bcb-bbfb-2eaac64ea3db.png)
