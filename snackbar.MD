
[snackvar in seperated function](https://flutter.dev/docs/release/breaking-changes/scaffold-messenger)

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