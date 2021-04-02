### alert dialog and snackbar

    import 'package:flutter/material.dart';
    import 'package:flutter/services.dart';

    void main() {
    runApp(MyApp());
    }

    class MyApp extends StatelessWidget {
    // This widget is the root of your application.
    @override
    Widget build(BuildContext context) {
        return MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
            primarySwatch: Colors.blue,
        ),
        home: MyHomePage(title: 'Flutter Demo Home Page'),
        );
    }
    }

    class MyHomePage extends StatefulWidget {
    MyHomePage({Key key, this.title}) : super(key: key);

    final String title;

    @override
    _MyHomePageState createState() => _MyHomePageState();
    }

    class _MyHomePageState extends State<MyHomePage> {
    TextEditingController controller = TextEditingController();

    Future<String> createAlertDialog(BuildContext context) {
        return showDialog(
            context: context,
            builder: (context) {
            return AlertDialog(
                title: Text("create your question"),
                content: TextField(
                controller: controller,
                keyboardType: TextInputType.name,
                ),
                actions: [
                MaterialButton(
                    color: Colors.red,
                    elevation: 5.0,
                    shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.all(Radius.circular(20))),
                    child: Text("Cancel"),
                    onPressed: () {
                        return Navigator.pop(context);
                    }),
                MaterialButton(
                    color: Colors.green,
                    elevation: 5.0,
                    shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.all(Radius.circular(20))),
                    child: Text("Submit"),
                    onPressed: () {
                        Navigator.of(context).pop(controller.text.toString());
                    })
                ],
            );
            });
    }

    showSnack(context, value) {
        // print(value);
        // ScaffoldMessenger(child: Text(value));
        ScaffoldMessenger.of(context).showSnackBar(SnackBar(
        content: Text("your question was $value"),
        duration: const Duration(seconds: 3),
        backgroundColor: Colors.green,
        shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(20))),
        // action: SnackBarAction(
        //   label: 'ACTION',
        //   onPressed: () {},
        // ),
        ));
    }

    @override
    Widget build(BuildContext context) {
        return Scaffold(
            appBar: AppBar(
            title: Text(widget.title),
            ),
            body: Center(
                child: FlatButton(
            color: Colors.deepOrange,
            textColor: Colors.white,
            shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.all(Radius.circular(20))),
            onPressed: () {
                createAlertDialog(context)
                    .then((value) => {showSnack(context, value)});
            },
            child: Text("modal"),
            )));
    }
    }
