import 'package:api_news/HomePage.dart';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'package:get/get.dart';

class PostApi extends StatefulWidget {
  const PostApi({super.key});

  @override
  State<PostApi> createState() => _PostApiState();
}

class _PostApiState extends State<PostApi> {
  TextEditingController Pass=TextEditingController();
  Future<void> _login() async {
    final String p = Pass.text.trim();
    try {
      final response = await http.post(
        Uri.parse("https://amazonboost.in/demo/api/login.php"),
        body: {
          'phone': p,
        },
      );

      if (response.statusCode == 200) {
        // Parse the response body as a JSON object
        final Map<String, dynamic> data = json.decode(response.body);

        // Check the status from the response
        if (data['status'] == 1) {
          // Successful login
          print(response.body);
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              behavior: SnackBarBehavior.floating,
              backgroundColor: Colors.green, // Change color as needed
              content: Text('Login Success'),
              duration: Duration(seconds: 2),
            ),
          );

         Navigator.push(context,MaterialPageRoute(builder: (context){
           return Home();
         }));
        } else {
          ScaffoldMessenger.of(context).showSnackBar(
              SnackBar(
                behavior: SnackBarBehavior.floating,
                backgroundColor: Colors.red, // Change color as needed
                content: Text('Login Failed'),
                duration: Duration(seconds: 2),
              ),);
        }
      } else {
        // Failed login due to non-200 status code
        print('Login Failed');
        // Handle error or show a message to the user
      }
    } catch (e) {
      // Handle network or server errors
      print('Error: $e');
    }
  }


  @override
  Widget build(BuildContext context) {

    return Scaffold(
      backgroundColor: Colors.white70,
      body: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextField(
              controller: Pass,
decoration: InputDecoration(
  hintText: "Gmail",

),
            ),

            SizedBox(height: 32,),
            ElevatedButton(
                onPressed:(){
              print(Pass.text);
              _login();},
                child: Center(child: Text("Login"))
            ) ],
        ),
      ),
    );
  }
}
