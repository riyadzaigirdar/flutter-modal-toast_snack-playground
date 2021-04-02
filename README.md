# modal

A new Flutter project.

## Getting Started

    FlatButton(
        color: Colors.deepOrange,
        textColor: Colors.white,
        shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(20))),
        onPressed: () {},
        child: Text("modal"),
    )

### Material button

    MaterialButton(
        color: Colors.red,
        elevation: 5.0,
        shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(20))),
        child: Text("Cancel"),
        onPressed: () {})

### snackbar example

    showSnack(context, value) {

        ScaffoldMessenger.of(context).showSnackBar(SnackBar(
        content: Text("your question was $value"),
        duration: const Duration(seconds: 3),
        backgroundColor: Colors.green,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.all(Radius.circular(50))),
        action: SnackBarAction(
           label: 'ACTION',
           onPressed: () {},
           ),
        ));
    }

# show bottom sheet
[flutter doc](https://api.flutter.dev/flutter/material/showModalBottomSheet.html)
    showBottomSheet() {
    return showModalBottomSheet<void>(
        context: context,
        builder: (BuildContext context) {
          return Container(
            height: 200,
            color: Colors.amber,
            child: Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                mainAxisSize: MainAxisSize.min,
                children: <Widget>[
                  const Text('Modal BottomSheet'),
                  ElevatedButton(
                    child: const Text('Close BottomSheet'),
                    onPressed: () {
                      Navigator.pop(context);
                      restoreStateToFactoryDefault();
                    },
                  )
                ],
              ),
            ),
          );
        });
    }

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

# modal alert snack with multifields(checkbox included) with validation key

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
    GlobalKey<FormState> _key = GlobalKey<FormState>();
    bool isChecked = false;
    TextEditingController controller = TextEditingController();

    Future createDialog(BuildContext context) async {
        return showDialog(
            context: context,
            builder: (context) {
            return StatefulBuilder(builder: (context, setState) {
                return AlertDialog(
                content: Form(
                    key: _key,
                    child: Column(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                        TextFormField(
                        validator: (value) {
                            return value.isNotEmpty ? null : "Enter Valid Email";
                        },
                        controller: controller,
                        decoration: InputDecoration(hintText: "Give your email"),
                        keyboardType: TextInputType.emailAddress,
                        keyboardAppearance: Brightness.light,
                        ),
                        Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                            Text("Is gsuite?"),
                            Checkbox(
                                value: isChecked,
                                onChanged: (checked) {
                                setState(() {
                                    isChecked = checked;
                                });
                                })
                        ],
                        )
                    ],
                    ),
                ),
                actions: <MaterialButton>[
                    MaterialButton(
                        child: Text("Okay"),
                        color: Colors.black,
                        shape: RoundedRectangleBorder(
                            borderRadius: BorderRadius.all(Radius.circular(20.0))),
                        onPressed: () {
                        if (_key.currentState.validate()) {
                            var val = {
                            "text": controller.text.toString(),
                            "checked": isChecked
                            };
                            Navigator.of(context).pop(val);
                        }
                        })
                ],
                );
            });
            });
    }

    showSnack(context, value) {
        ScaffoldMessenger.of(context).showSnackBar(SnackBar(
        content: Text(
            "your email was ${value["text"]} and check was ${value["checked"]}",
            style: TextStyle(color: Colors.white),
        ),
        duration: const Duration(seconds: 3),
        backgroundColor: Colors.black,
        shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.all(Radius.circular(50))),
        action: SnackBarAction(
            label: 'ACTION',
            textColor: Colors.white,
            onPressed: () {},
        ),
        ));
        controller.text = "";
        isChecked = false;
        setState(() {});
    }

    @override
    Widget build(BuildContext context) {
        return Scaffold(
        appBar: AppBar(
            title: Align(
                alignment: Alignment.centerLeft,
                child: Text("Modal with multiple fields")),
            actions: <IconButton>[
            IconButton(icon: Icon(Icons.account_box), onPressed: () {})
            ],
        ),
        body: Center(
            child: MaterialButton(
            color: Colors.red,
            textColor: Colors.white,
            shape: RoundedRectangleBorder(
                borderRadius: BorderRadius.all(Radius.circular(20.0))),
            child: Text("popup"),
            onPressed: () async {
                // createDialog(context).then((value) => {print(value)});
                var result = await createDialog(context);
                showSnack(context, result);
            },
            ),
        ),
        );
    }
    }
