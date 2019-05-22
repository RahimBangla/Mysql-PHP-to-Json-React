
# React Native Connect Run Apps Using Localhost PhpMyAdmin MySQL Database System

Contents in this project Connect Run Apps Using Localhost PhpMyAdmin MySQL Database System in React Native iOS Android:
1. Before getting started we need to download and install the latest version on XAMPP software, This is very important for present tutorial because XAMPP gives the ability to our system by making and working like a Localhost computer, Here is the link.

2. After successfully downloaded we need to install it like any other normal software, Now after done installing start the XAMPP and start Apache and MySQL services . Now your system is ready to use like a local host computer.




 3. Creating MySQL Database and Table for application :
1. Open your web browser(Mozilla Firefox or Google chrome) and type  localhost/phpmyadmin in address bar to open the MySQL database panel.

2.Click on the New link to make a new MySQL database on your local system.


 
3. Now enter the database name as Flowers and hit the Create button.4. Now our database is successfully created, Next step is to crate table in your MySQL database. So select your newly created database from side panel.5. Enter table name as flowers_name and select column as 2 .6. Now we need to enter the table column names, column data type, length and select id as primary key with Auto increment.7. Here you go now the table has been created successfully, Now we need to insert some data inside it. So select your table and click on Insert button.8. Insert some records as i did in below screenshot and hit the Go button.

Screenshot of Table after inserting some records:

4. Create PHP Script to Convert MySQL database data into JSON :

Next step is to create 2 PHP files named as DBConfig.php and FlowersList.php .


 
DBConfig.php : This file contain all the useful information like Database name, database username, database password and database host name.

FlowersList.php : This file would convert the MySQL database data into JSON format.

Code for DBConfig.php file:

<?php
 
//Define your host here.
$HostName = "localhost";
 
//Define your database name here.
$DatabaseName = "flowers";
 
//Define your database username here.
$HostUser = "root";
 
//Define your database password here.
$HostPass = "";
 
?>














<?php
 
//Define your host here.
$HostName = "localhost";
 
//Define your database name here.
$DatabaseName = "flowers";
 
//Define your database username here.
$HostUser = "root";
 
//Define your database password here.
$HostPass = "";
 
?>
Code for FlowersList.php file:

<?php
include 'DBConfig.php';
 
// Create connection
$conn = new mysqli($HostName, $HostUser, $HostPass, $DatabaseName);
 
if ($conn->connect_error) {
 
 die("Connection failed: " . $conn->connect_error);
} 
 
$sql = "SELECT * FROM flowers_name";
 
$result = $conn->query($sql);
 
if ($result->num_rows >0) {
 
 
 while($row[] = $result->fetch_assoc()) {
 
 $tem = $row;
 
 $json = json_encode($tem);
 
 
 }
 
} else {
 echo "No Results Found.";
}
 echo $json;
$conn->close();
?>










<?php
include 'DBConfig.php';
 
// Create connection
$conn = new mysqli($HostName, $HostUser, $HostPass, $DatabaseName);
 
if ($conn->connect_error) {
 
 die("Connection failed: " . $conn->connect_error);
} 
 
$sql = "SELECT * FROM flowers_name";
 
$result = $conn->query($sql);
 
if ($result->num_rows >0) {
 
 
 while($row[] = $result->fetch_assoc()) {
 
 $tem = $row;
 
 $json = json_encode($tem);
 
 
 }
 
} else {
 echo "No Results Found.";
}
 echo $json;
$conn->close();
?>
5. Now we need to make a folder named as Flowers_Site inside C:\xampp\htdocs\ folder.

6. Copy both DBConfig.php and FlowersList.php file inside the Flowers_Site folder.


 


8. Here you go, next step is to open the Command Prompt and execute ipconfig command. This command would show us our local computer system IP Address.

Note : If you are connected to WiFi then select the IPv4 address under Wireless LAN adapter Wi-Fi. If you are connected to LAN then selected IPv4 address under Ethernet adapter.

9. After getting the IP Address and copied all the files into the folder our JSON is ready to parse with react native app but we need to make sure the URL is working correctly so we need to open the URL in our web browser  http://192.168.2.103/Flowers_Site/FlowersList.php , This is my URL you need to put your local IP address here. We would use this URL in our react native app.

10. Start Coding for App :

1. Import StyleSheet, ActivityIndicator, ListView, Text, View and Alert component in your project.

import React, { Component } from 'react';
 
import { StyleSheet, ActivityIndicator, ListView, Text, View, Alert } from 'react-native';
1
2
3
import React, { Component } from 'react';
 
import { StyleSheet, ActivityIndicator, ListView, Text, View, Alert } from 'react-native';
2. Create constructor() in your project and make a State named as isLoading with true Boolean value, This state is used to show Activity loading indicator while loading data from server.

constructor(props) {

    super(props);

    this.state = {

      isLoading: true,

    }

  }















constructor(props) {
 
    super(props);
 
    this.state = {
 
      isLoading: true,
 
    }
 
  }
3. Create componentDidMount() method and inside it we would make the fetch API to parse JSON from local server.

componentDidMount(){

  return fetch('http://192.168.2.103/Flowers_Site/FlowersList.php')
  .then((response) => response.json())
  .then((responseJson) => {
    let ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
    this.setState({
      isLoading: false,
      dataSource: ds.cloneWithRows(responseJson),
    }, function() {
      // In this block you can do something with new state.
    });
  })
  .catch((error) => {
    console.error(error);
  });

}









componentDidMount(){
 
  return fetch('http://192.168.2.103/Flowers_Site/FlowersList.php')
  .then((response) => response.json())
  .then((responseJson) => {
    let ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
    this.setState({
      isLoading: false,
      dataSource: ds.cloneWithRows(responseJson),
    }, function() {
      // In this block you can do something with new state.
    });
  })
  .catch((error) => {
    console.error(error);
  });
 
}
4. Create a function named as GetItem() to show the selected flower name when user click the ListView item.

GetItem (flower_name) {
   
  Alert.alert(flower_name);
 
}
1
2
3
4
5
GetItem (flower_name) {
   
  Alert.alert(flower_name);
 
}
5. Create another function named as ListViewItemSeparator(), This function would render a horizontal line between each ListView element.

ListViewItemSeparator = () => {
    return (
      <View
        style={{
          height: 2,
          width: "100%",
          backgroundColor: "#000",
        }}
      />
    );
  }
1
2
3
4
5
6
7
8
9
10
11
ListViewItemSeparator = () => {
    return (
      <View
        style={{
          height: 2,
          width: "100%",
          backgroundColor: "#000",
        }}
      />
    );
  }
6. Put a IF condition inside render’s block, using this we would show the Activity Indicator with State value.

render() {
    if (this.state.isLoading) {
      return (
        <View style={{flex: 1, paddingTop: 20}}>
          <ActivityIndicator />
        </View>
      );
    }
 
    return (
 
     
    );
  }
1
2
3
4
5
6
7
8
9
10
11
12
13
14
render() {
    if (this.state.isLoading) {
      return (
        <View style={{flex: 1, paddingTop: 20}}>
          <ActivityIndicator />
        </View>
      );
    }
 
    return (
 
     
    );
  }
7. Creating the ListView component inside the render’s return block.

render() {
    if (this.state.isLoading) {
      return (
        <View style={{flex: 1, paddingTop: 20}}>
          <ActivityIndicator />
        </View>
      );
    }
 
    return (
 
      <View style={styles.MainContainer}>
 
        <ListView
 
          dataSource={this.state.dataSource}
 
          renderSeparator= {this.ListViewItemSeparator}

          enableEmptySections = {true}
 
          renderRow={(rowData) => <Text style={styles.rowViewContainer} 
          onPress={this.GetItem.bind(this, rowData.flower_name)} >{rowData.flower_name}</Text>}

        />
 
      </View>
    );
  }
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
render() {
    if (this.state.isLoading) {
      return (
        <View style={{flex: 1, paddingTop: 20}}>
          <ActivityIndicator />
        </View>
      );
    }
 
    return (
 
      <View style={styles.MainContainer}>
 
        <ListView
 
          dataSource={this.state.dataSource}
 
          renderSeparator= {this.ListViewItemSeparator}
 
          enableEmptySections = {true}
 
          renderRow={(rowData) => <Text style={styles.rowViewContainer} 
          onPress={this.GetItem.bind(this, rowData.flower_name)} >{rowData.flower_name}</Text>}
 
        />
 
      </View>
    );
  }
8. Creating Style.

const styles = StyleSheet.create({
 
MainContainer :{
justifyContent: 'center',
flex:1,
margin: 10
 
},
 
rowViewContainer: {
  fontSize: 20,
  paddingRight: 10,
  paddingTop: 10,
  paddingBottom: 10,
}
 
});
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
const styles = StyleSheet.create({
 
MainContainer :{
justifyContent: 'center',
flex:1,
margin: 10
 
},
 
rowViewContainer: {
  fontSize: 20,
  paddingRight: 10,
  paddingTop: 10,
  paddingBottom: 10,
}
 
});
Complete source code for App.js File :

import React, { Component } from 'react';
 
import { StyleSheet, ActivityIndicator, ListView, Text, View, Alert } from 'react-native';
 
export default class Project extends Component {
  
  constructor(props) {

    super(props);

    this.state = {

      isLoading: true,

    }

  }

GetItem (flower_name) {
   
  Alert.alert(flower_name);
 
}
 
componentDidMount(){

  return fetch('http://192.168.2.103/Flowers_Site/FlowersList.php')
  .then((response) => response.json())
  .then((responseJson) => {
    let ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
    this.setState({
      isLoading: false,
      dataSource: ds.cloneWithRows(responseJson),
    }, function() {
      // In this block you can do something with new state.
    });
  })
  .catch((error) => {
    console.error(error);
  });

}
 
  ListViewItemSeparator = () => {
    return (
      <View
        style={{
          height: 2,
          width: "100%",
          backgroundColor: "#000",
        }}
      />
    );
  }

  render() {
    if (this.state.isLoading) {
      return (
        <View style={{flex: 1, paddingTop: 20}}>
          <ActivityIndicator />
        </View>
      );
    }
 
    return (
 
      <View style={styles.MainContainer}>
 
        <ListView
 
          dataSource={this.state.dataSource}
 
          renderSeparator= {this.ListViewItemSeparator}

          enableEmptySections = {true}
 
          renderRow={(rowData) => <Text style={styles.rowViewContainer} 
          onPress={this.GetItem.bind(this, rowData.flower_name)} >{rowData.flower_name}</Text>}

        />
 
      </View>
    );
  }
}
 
const styles = StyleSheet.create({
 
MainContainer :{
justifyContent: 'center',
flex:1,
margin: 10
 
},
 
rowViewContainer: {
  fontSize: 20,
  paddingRight: 10,
  paddingTop: 10,
  paddingBottom: 10,
}
 
});
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99
100
101
102
103
import React, { Component } from 'react';
 
import { StyleSheet, ActivityIndicator, ListView, Text, View, Alert } from 'react-native';
 
export default class Project extends Component {
  
  constructor(props) {
 
    super(props);
 
    this.state = {
 
      isLoading: true,
 
    }
 
  }
 
GetItem (flower_name) {
   
  Alert.alert(flower_name);
 
}
 
componentDidMount(){
 
  return fetch('http://192.168.2.103/Flowers_Site/FlowersList.php')
  .then((response) => response.json())
  .then((responseJson) => {
    let ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
    this.setState({
      isLoading: false,
      dataSource: ds.cloneWithRows(responseJson),
    }, function() {
      // In this block you can do something with new state.
    });
  })
  .catch((error) => {
    console.error(error);
  });
 
}
 
  ListViewItemSeparator = () => {
    return (
      <View
        style={{
          height: 2,
          width: "100%",
          backgroundColor: "#000",
        }}
      />
    );
  }
 
  render() {
    if (this.state.isLoading) {
      return (
        <View style={{flex: 1, paddingTop: 20}}>
          <ActivityIndicator />
        </View>
      );
    }
 
    return (
 
      <View style={styles.MainContainer}>
 
        <ListView
 
          dataSource={this.state.dataSource}
 
          renderSeparator= {this.ListViewItemSeparator}
 
          enableEmptySections = {true}
 
          renderRow={(rowData) => <Text style={styles.rowViewContainer} 
          onPress={this.GetItem.bind(this, rowData.flower_name)} >{rowData.flower_name}</Text>}
 
        />
 
      </View>
    );
  }
}
 
const styles = StyleSheet.create({
 
MainContainer :{
justifyContent: 'center',
flex:1,
margin: 10
 
},
 
rowViewContainer: {
  fontSize: 20,
  paddingRight: 10,
  paddingTop: 10,
  paddingBottom: 10,
}
 
});
