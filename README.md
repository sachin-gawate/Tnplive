# Tnplive Demo Code 
Tnplive Android App Developed in Flutter
import 'dart:async';
import 'dart:convert';
import 'dart:io';

import 'package:TnpLive/data/rest_ds.dart';
import 'package:TnpLive/models/educationDetails.dart';
import 'package:TnpLive/pages/auth.dart';
import 'package:TnpLive/pages/education_details_add_pages/AddAwards.dart';
import 'package:TnpLive/pages/education_details_add_pages/AddCareerObjective.dart';

class EducationDetails extends StatefulWidget {
  @override
  EducationDetailsState createState() => EducationDetailsState();

  String studentId;

  String email;

  EducationDetails({this.studentId});
}

class EducationDetailsState extends State<EducationDetails> {
  String _id;

  String _degree;

  String _branch;

  Auth auth = Auth();

  StudentCurrEducation _details;
  bool loading;
  RestDatasource api = new RestDatasource();
  String skillId;
  String langId;

//  Future<bool> _onBackPressed() {
//
////    Navigator.of(context).pop(MyHomePage());
//
//    Navigator.of(context).push(MaterialPageRoute(
//        builder: (ctx) => MyHomePage()
//    ));
//
//  }

  var loder = new Map();
  var user1 = new Map();
  var user = new Map();
  var degree = new Map();
  var branch = new Map();
  var studentObjective = new Map();
  var college = new Map();
  var semstatus = new Map();
  String collegename;
  String co, coid;

  String _semister;
  String _semisternumber;

  String _aggregateper;
  String _straggregatePercentage;
  String _strduration;

  String startYear;
  String endYear;
  String lateralentry;

  String _university;
  String educationgapyear;
  String educationGap;
  String resume;
  String Certificationsid;
  String Skillsdeleteid;

  ProgressDialog pr;

  String _resume;

  var semesterMarks = new List();

  Map data1 = new Map();
  Map data = new Map();

  List<SemisterList> semisterList = new List();
  List<PrevEducationList> prevEducationList = new List();

  List<StudentSkillsList> studentSkillsList = new List();
  List<StudentLanguagesList> studentLanguagesList = new List();

  List<StudentTestsList> studentTestsList = new List();
  List<StudentPatentsList> studentPatentsList = new List();

  List<StudentProjectsList> studentProjectsList = new List();
  List<StudentAwardsList> studentAwardsList = new List();

  List<StudentCertificationsList> studentCertificationsList = new List();
  List<StudentConferencesList> studentConferencesList = new List();
  List<StudentCompetitionsList> studentCompetitionsList = new List();

  List<StudentPublicationsList> studentPublicationsList = new List();
  List<StudentScholarshipsList> studentScholarshipsList = new List();
  List<StudentCurriculumsList> studentCurriculumsList = new List();

  List<StudentInternshipList> studentInternshipList = new List();
  final GlobalKey<RefreshIndicatorState> _refreshIndicatorKey =
      new GlobalKey<RefreshIndicatorState>();

  Timer timer;
  int counter = 0;

  List<StateListItem> stateList = new List();

  Map Data = new Map();

  //  Map userMap =new Map;
  String _sem1;
  String _sem2;
  String _sem3;
  String _sem4;
  String _sem5;
  String _sem6;
  String _sem7;
  String _sem8;

  String AwardsId;

  String CurriculumId;
  String Competitionsid;
  String Conferences_workshopsid;
  String Patentsid;
  String PreviousEducationid;
  String Projectid;
  String Publicationsid;
  String Scholarshipsid;
  String Test_scoresid;
  String StudentInternshipid;
  String langid;

  String studentObjectiveid;

  @override
  Future initState() {
    super.initState();
    print('${widget.studentId}');

//    _refresh();

    createVerifi(widget.studentId);

    createEduDetails(widget.studentId);

//    timer = Timer.periodic(Duration(seconds: 3), (Timer t) =>   createEduDetailsfresh(widget.studentId));

    setState(() {
//      createEduDetails(widget.studentId);
    });
  }

  void addValue() {
    setState(() {
      counter++;
    });
  }

  @override
  void dispose() {
//    timer?.cancel();
    super.dispose();
  }

  Future<void> createVerifi(String id) async {
//    final prefs = await SharedPreferences.getInstance();
//    final String id = prefs.getString("id");
    final prefs = await SharedPreferences.getInstance();
//    final String id = prefs.getString("id");
    final http.Response response = await http.post(
      'https://tnplive.com/mobi/studentProfile',
      headers: <String, String>{'Content-Type': 'application/json'},
      body: jsonEncode(<String, String>{
        "studentId": prefs.getString("id"),
      }),
    );

    if (response.statusCode == 200) {
      String responseString = response.body;

      print("the id is" + responseString);
      data1 = json.decode(response.body);

      data1['resObject'].forEach((k, v) => user1[k] = v);
    }
  }

  ///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

  Future<void> createEduDetails(String id) async {
    final http.Response response = await http.post(
      'https://tnplive.com/mobi/educationDetails',
      headers: <String, String>{'Content-Type': 'application/json'},
      body: jsonEncode(<String, String>{
        "studentId": id,
      }),
    );

    if (response.statusCode == 200) {
      String responseString = response.body;

      print("the id is" + responseString);

      data = json.decode(response.body);

//      LangList();

//
//      _email= data['resStatus'];
//      data['resStatus'];

      data['resObject']['studentCurrEducation'].forEach((k, v) => user[k] = v);

      String id = '${user['id']}';
      educationgapyear = '${user['educationgapyear']}';
      String semisternumber = '${user['semester']}';


      String resume = '${user['resume']}';


      if (semisternumber == "null") {
        semisternumber = "-";
      }

      String semister = semisternumber;

      print(semister);

      String aggregateper = '${user['aggregate']}';
      String university = '${user['university']}';

      //////////////////////////degree////////////////////

      String degreename;

      if (data['resObject']['studentCurrEducation']['degree'] == null) {
        degreename = "-";
      } else {
        data['resObject']['studentCurrEducation']['degree']
            .forEach((k, v) => degree[k] = v);

        degreename = '${degree['name']}';
      }

      print('degree OK: ${branch['name']}');

      String branchname;

      if (data['resObject']['studentCurrEducation']['branch'] == null) {
        branchname = "";
      } else {
        data['resObject']['studentCurrEducation']['branch']
            .forEach((k, v) => branch[k] = v);
        branchname = '${branch['name']}';
      }

      String straggregatePercentage = '${user['aggregatePercentage']}';

      String startyears = '${user['duration']}';

      if (startyears == "null") {
        startyears = "- ";
      }

      String strduration = startyears + "-" + '${user['graduationyear']}';

      startYear = '${user['duration']}';
      endYear = '${user['graduationyear']}';
      lateralentry = '${user['lateralentry']}';
      educationGap = '${user['educationGap']}';
      resume = '${user['resume']}';

      if (degreename == "null") {
        degreename = "-";
      }
      if (branchname == "null") {
        branchname = "-";
      }
      if (aggregateper == "null") {
        aggregateper = "-";
      }
      if (straggregatePercentage == "null") {
        straggregatePercentage = "-";
      }
      if (straggregatePercentage == "null") {
        straggregatePercentage = "-";
      }

      _id = id;
      _semister = semister;
      _semisternumber = semisternumber;
      _degree = degreename;
      _branch = branchname;

      _aggregateper = aggregateper;
      _straggregatePercentage = straggregatePercentage;
      _strduration = strduration;
      _university = university;

      //////////////////////////degree////////////////////
      data['resObject']['studentObjective']
          .forEach((k, v) => studentObjective[k] = v);

      print('degree OK: ${studentObjective['objective']}');
      //////////////////////////college////////////////////
      data['resObject']['studentCurrEducation']['college']
          .forEach((k, v) => college[k] = v);

      print('degree OK: ${college['collegename']}');
      collegename = ('${college['collegename']}');

      co = '${studentObjective['objective']}';
      studentObjectiveid = '${studentObjective['id']}';

      coid = '${college['id']}';

      print(studentObjective);

      data['resObject'].forEach((k, v) => semstatus[k] = v);

      String semsterStatus = '${semstatus['semsterStatus']}';

      print(
          "_aggregateper=aggregateper,_straggregatePercentage=straggregatePercentage,_strduration=strdurationcccccccccc" +
              semsterStatus);

      setState(() {
        _id = id;
        _semister = semister;
        _resume=resume;
        _semisternumber = semisternumber;
        _degree = degreename;
        _branch = branchname;
        _aggregateper = aggregateper;
        _straggregatePercentage = straggregatePercentage;
        _strduration = strduration;
        _university = university;
      });

//      collegename
//      _university
//      _degree
//      _branch
//         startYear
//         endYear
//      _aggregateper
//      _straggregatePercentage
//      _semister

//      setState(() =>
//      {
//        _semister = semister,_degree =degreename ,
//        _aggregateper=aggregateper,_straggregatePercentage=straggregatePercentage,_strduration=strduration
//
//      });

      ///////////////////////////////////semesterMarks//////////////////////////

      data["resObject"]['semesterMarks'].forEach((item) {
        semisterList.add(SemisterList(
            id: '${item['id']}',
            studentId: '${item['studentId']}',
            semester: '${item['semester']}',
            sgpa: '${item['sgpa']}',
            percentage: '${item['percentage']}',
            curr_session: '${item['curr_session']}',
            backlog: '${item['backlog']}',
            ongoingbacklogs: '${item['ongoingbacklogs']}'));
      });
      ///////////////////////////////////prevEducation//////////////////////////
      data["resObject"]['prevEducation'].forEach((item) {
        prevEducationList.add(PrevEducationList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          degree: '${item['degree']}',
          educationcenter: '${item['educationcenter']}',
          educationboard: '${item['educationboard']}',
          grade: '${item['grade']}',
          percentage: '${item['percentage']}',
          cgpa: '${item['cgpa']}',
          startyear: '${item['startyear']}',
          endyear: '${item['endyear']}',
        ));
      });

      ///////////////////////////////////prevEducation//////////////////////////
      data["resObject"]['studentProjects'].forEach((item) {
        studentProjectsList.add(StudentProjectsList(
            id: '${item['id']}',
            studentId: '${item['studentId']}',
            title: '${item['title']}',
            skill: '${item['skill']}',
            start: '${item['start']}',
            end: '${item['end']}',
            college: '${item['college']}',
            description: '${item['description']}'));
      });
      ///////////////////////////////////studentAwards//////////////////////////
      data["resObject"]['studentAwards'].forEach((item) {
        studentAwardsList.add(StudentAwardsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          issuer: '${item['issuer']}',
          issueyear: '${item['issueyear']}',
        ));
      });
      ///////////////////////////////////studentAwards//////////////////////////
      data["resObject"]['studentCertifications'].forEach((item) {
        studentCertificationsList.add(StudentCertificationsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          name: '${item['name']}',
          authority: '${item['authority']}',
          url: '${item['url']}',
          licence: '${item['licence']}',
        ));
      });
      ///////////////////////////////////studentAwards//////////////////////////
      data["resObject"]['studentCompetitions'].forEach((item) {
        studentCompetitionsList.add(StudentCompetitionsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          position: '${item['position']}',
          year: '${item['year']}',
          college: '${item['college']}',
        ));
      });
      ///////////////////////////////////studentAwards//////////////////////////
      data["resObject"]['studentConferences'].forEach((item) {
        studentConferencesList.add(StudentConferencesList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          organizer: '${item['organizer']}',
          year: '${item['year']}',
          address: '${item['address']}',
          description: '${item['description']}',
        ));
      });

      ///////////////////////////////////studentPublications//////////////////////////
      data["resObject"]['studentPublications'].forEach((item) {
        studentPublicationsList.add(StudentPublicationsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          publisher: '${item['publisher']}',
          url: '${item['url']}',
          year: '${item['year']}',
          description: '${item['description']}',
        ));
      });

      ///////////////////////////////////studentScholarships//////////////////////////
      data["resObject"]['studentScholarships'].forEach((item) {
        studentScholarshipsList.add(StudentScholarshipsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          year: '${item['year']}',
          associated: '${item['associated']}',
          description: '${item['description']}',
        ));
      });

      ///////////////////////////////////studentCurriculums//////////////////////////
      data["resObject"]['studentCurriculums'].forEach((item) {
        studentCurriculumsList.add(StudentCurriculumsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          description: '${item['description']}',
        ));
      });

      ///////////////////////////////////studentSkillsList//////////////////////////
      data["resObject"]['studentSkills'].forEach((item) {
        studentSkillsList.add(StudentSkillsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          skill: '${item['skill']}',
          proficiency: '${item['proficiency']}',
        ));
      });

      ///////////////////////////////////studentLanguagesList//////////////////////////
      data["resObject"]['studentLanguages'].forEach((item) {
        studentLanguagesList.add(StudentLanguagesList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          language: '${item['language']}',
          proficiency: '${item['proficiency']}',
        ));
      });

      ///////////////////////////////////studentTestsList//////////////////////////
      data["resObject"]['studentTests'].forEach((item) {
        studentTestsList.add(StudentTestsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          year: '${item['year']}',
          obtain: '${item['obtain']}',
          total: '${item['total']}',
        ));
      });

      ///////////////////////////////////studentPatentsList//////////////////////////
      data["resObject"]['studentPatents'].forEach((item) {
        studentPatentsList.add(StudentPatentsList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          office: '${item['office']}',
          app_number: '${item['app_number']}',
          status: '${item['status']}',
          date: '${item['date']}',
          description: '${item['description']}',
        ));
      });

      ///////////////////////////////////studentPatentsList//////////////////////////
      data["resObject"]['studentInternship'].forEach((item) {
        studentInternshipList.add(StudentInternshipList(
          id: '${item['id']}',
          studentId: '${item['studentId']}',
          title: '${item['title']}',
          company: '${item['company']}',
          location: '${item['location']}',
          start: '${item['start']}',
          end: '${item['end']}',
          salary: '${item['salary']}',
          description: '${item['description']}',
        ));
      });
    }

   String sem1;
   String sem2;
   String sem3;
   String sem4;
   String sem5;
   String sem6;
   String sem7;
   String sem8;
    for (var index = 0; index < semisterList.length; index++) {
      if (semisterList[index].semester == "1") {
        sem1 = "1";
      } else
      if (semisterList[index].semester == "2") {
        sem2 = "2";
      } else
      if (semisterList[index].semester == "3") {
        sem3 = "3";
      } else
      if (semisterList[index].semester == "4") {
        sem4 = "4";
      } else
      if (semisterList[index].semester == "5") {
        sem5 = "5";
      } else
      if (semisterList[index].semester == "6") {
        sem6 = "6";
      } else
      if (semisterList[index].semester == "7") {
        sem7 = "7";
      } else
      if (semisterList[index].semester == "8") {
        sem8 = "8";
      } else {
        sem1="0";
        sem2="0";
        sem3="0";
        sem4="0";
        sem5="0";
        sem6="0";
        sem7="0";
        sem8="0";
      }

      setState(() {
        _id = id;
        _sem1=sem1;
        _sem2=sem2;
        _sem3=sem3;
        _sem4=sem4;
        _sem5=sem5;
        _sem6=sem6;
        _sem7=sem7;
        _sem8=sem8;
      });

      // Found the person, stop the
    }
  }

  @override
  Widget build(BuildContext context) {
    Future<bool> _onBackPressed() {
      var route = new MaterialPageRoute(
        builder: (BuildContext context) =>
            new MyHomePage(studentId: widget.studentId),
      );
      Navigator.pop(context);
      Navigator.of(context).push(route);
    }

    pr = new ProgressDialog(context,
        type: ProgressDialogType.Normal, isDismissible: true, showLogs: true);

    pr.style(
      progress: 50.0,
      message: "Please wait...",
      progressWidget: Container(
          padding: EdgeInsets.all(8.0), child: CircularProgressIndicator()),
      maxProgress: 100.0,
      progressTextStyle: TextStyle(
          color: Colors.black, fontSize: 13.0, fontWeight: FontWeight.w400),
      messageTextStyle: TextStyle(
          color: Colors.black, fontSize: 19.0, fontWeight: FontWeight.w600),
    );
    return WillPopScope(
        onWillPop: _onBackPressed,
        child: Scaffold(
            appBar: AppBar(
              backgroundColor: MyColors.bkColor,
              elevation: 0.0,
              title: Text(
                // '${widget.name[0].toUpperCase()}${widget.name.substring(1)}',
                'Education Details',
                style: TextStyle(
                    fontSize: 24.0,
                    color: Colors.white,
                    fontWeight: FontWeight.bold),
              ),
              leading: IconButton(
                icon: Icon(
                  Platform.isIOS ? Icons.arrow_back_ios : Icons.arrow_back,
                  color: Colors.white,
                ),
                onPressed: () {
                  var route = new MaterialPageRoute(
                    builder: (BuildContext context) =>
                        new MyHomePage(studentId: _id),
                  );
                  Navigator.pop(context);
                  Navigator.of(context).push(route);
                },
                iconSize: 26.0,
              ),
              centerTitle: false,
            ),

//        floatingActionButton: FloatingActionButton(
//          child: Icon(Platform.isIOS ? Icons.arrow_back_ios : Icons.arrow_back),
//          onPressed: () {
//            Navigator.pop(context);
//          },
//        ),

//        backgroundColor: Color.fromRGBO(255, 255, 255, .9),

            body: user.isEmpty
                ? Center(
                    child: CircularProgressIndicator(),
                  )
                : RefreshIndicator(
                    key: _refreshIndicatorKey,
                    onRefresh: _refresh,
                    child: Padding(
                      padding: const EdgeInsets.only(
                          left: 10, right: 10, top: 10, bottom: 10),
                      child: Card(
                        child: ListView(
                          padding: EdgeInsets.only(
                              left: 10, right: 10, bottom: 0, top: 10),
                          children: <Widget>[
                            Stack(
                              children: <Widget>[
                                Padding(
                                  padding: const EdgeInsets.all(10.0),
                                  child: Column(
                                    children: <Widget>[
                                      '${user1['verify']}' == "true"
                                          ? Container(
                                              margin: const EdgeInsets.only(
                                                  top: 14, bottom: 10),
                                              child: RaisedButton(

                                                  color: Colors.green,
                                                  padding: EdgeInsets.symmetric(
                                                      horizontal: 40.0, vertical: 5.0),
                                                  shape: RoundedRectangleBorder(
                                                      side: BorderSide(
                                                          color: Colors.green,
                                                          width: 1,
                                                          style: BorderStyle.solid),
                                                      borderRadius:
                                                      BorderRadius.circular(30)),
                                                  child: Padding(
                                                    padding: const EdgeInsets.all(0.0),
                                                    child: Card(
                                                      color: Colors.white,
                                                      shape: RoundedRectangleBorder(
                                                          borderRadius:
                                                          BorderRadius.circular(
                                                              50.0)),
                                                      child: Padding(
                                                        padding:
                                                        const EdgeInsets.all(0.0),
                                                        child: Row(
                                                          // Replace with a Row for horizontal icon + text
                                                          children: <Widget>[
                                                            Padding(
                                                              padding:
                                                              const EdgeInsets.all(
                                                                  8.0),
                                                              child: Image.asset(
                                                                  'assets/images/profile_verified.png',
                                                                  width: 30.0,
                                                                  height: 30.0),
                                                            ),
                                                            Text(
                                                              'Verified!',
                                                              style: TextStyle(
                                                                  fontSize: 14.0,
                                                                  color:
                                                                  MyColors.bkColor,
                                                                  fontWeight:
                                                                  FontWeight.bold),
                                                            ),
                                                          ],
                                                        ),
                                                      ),
                                                    ),
                                                  ),
                                                  onPressed: () {
                                                    // If the form is valid, display a snackbar. In the real world,
                                                    // you'd often call a server or save the information in a database.
//                                              _buildGoogleSignInButton();
                                                  }),
                                            )
                                          : '${user1['verifyRequest']}' ==
                                                  "true"
                                              ? Container(
                                                  margin: const EdgeInsets.only(
                                                      top: 14, bottom: 10),
                                                  child: RaisedButton(


                                                      color: Colors.green[200],
                                                      padding: EdgeInsets.symmetric(
                                                          horizontal: 40.0, vertical: 5.0),
                                                      shape: RoundedRectangleBorder(
                                                          side: BorderSide(
                                                              color: Colors.green,
                                                              width: 1,
                                                              style: BorderStyle.solid),
                                                          borderRadius:
                                                          BorderRadius.circular(30)),
                                                      child: Padding(
                                                        padding: const EdgeInsets.all(0.0),
                                                        child: Card(
                                                          color: Colors.white,
                                                          shape: RoundedRectangleBorder(
                                                              borderRadius:
                                                              BorderRadius.circular(
                                                                  50.0)),
                                                          child: Padding(
                                                            padding:
                                                            const EdgeInsets.all(0.0),
                                                            child: Row(
                                                              // Replace with a Row for horizontal icon + text
                                                              children: <Widget>[
                                                                Padding(
                                                                  padding:
                                                                  const EdgeInsets.all(
                                                                      8.0),
                                                                  child: Image.asset(
                                                                      'assets/images/request_sent.png',
                                                                      width: 30.0,
                                                                      height: 30.0),
                                                                ),
                                                                Text(
                                                                  'Request sent!',
                                                                  style: TextStyle(
                                                                      fontSize: 14.0,
                                                                      color:
                                                                      MyColors.bkColor,
                                                                      fontWeight:
                                                                      FontWeight.bold),
                                                                ),
                                                              ],
                                                            ),
                                                          ),
                                                        ),
                                                      ),


                                                      onPressed: () {
                                                        // If the form is valid, display a snackbar. In the real world,
                                                        // you'd often call a server or save the information in a database.
//                                          _buildGoogleSignInButton();
                                                      }),
                                                )
                                              : Container(
                                                  width: double.infinity,
                                                  margin: const EdgeInsets.only(
                                                      top: 14, bottom: 10),
                                                  child: RaisedButton(


                                                      color: Colors.red[200],
                                                      padding: EdgeInsets.symmetric(
                                                          horizontal: 10.0, vertical: 5.0),
                                                      shape: RoundedRectangleBorder(
                                                          side: BorderSide(
                                                              color: Colors.red,
                                                              width: 1,
                                                              style: BorderStyle.solid),
                                                          borderRadius:
                                                          BorderRadius.circular(30)),
                                                      child: Padding(
                                                        padding: const EdgeInsets.all(0.0),
                                                        child: Card(
                                                          color: Colors.white,
                                                          shape: RoundedRectangleBorder(
                                                              borderRadius:
                                                              BorderRadius.circular(
                                                                  50.0)),
                                                          child: Padding(
                                                            padding:
                                                            const EdgeInsets.all(0.0),
                                                            child: Row(
                                                              // Replace with a Row for horizontal icon + text
                                                              children: <Widget>[
                                                                Padding(
                                                                  padding:
                                                                  const EdgeInsets.all(
                                                                      8.0),
                                                                  child: Image.asset(
                                                                      'assets/images/send_verification_request.png',
                                                                      width: 30.0,
                                                                      height: 30.0),
                                                                ),
                                                                Text(
                                                                  'Send verification request!',
                                                                  style: TextStyle(
                                                                      fontSize: 14.0,
                                                                      color:
                                                                      MyColors.bkColor,
                                                                      fontWeight:
                                                                      FontWeight.bold),
                                                                ),
                                                              ],
                                                            ),
                                                          ),
                                                        ),
                                                      ),

                                                      onPressed: () {
                                                        // If the form is valid, display a snackbar. In the real world,
                                                        // you'd often call a server or save the information in a database.
                                                        _buildGoogleSignInButton();
                                                      }),
                                                ),

                                      Container(
                                        child: Card(
                                          color: Colors.white,
                                          elevation: 0.0,
                                          shape: RoundedRectangleBorder(
                                              borderRadius:
                                                  BorderRadius.circular(15.0)),
                                          child: InkWell(
                                            child: Padding(
                                              padding: const EdgeInsets.only(
                                                  left: 10.0,
                                                  top: 10.0,
                                                  right: 10.0,
                                                  bottom: 10.0),
                                              child: Column(
                                                children: <Widget>[
                                                  Row(
                                                    mainAxisAlignment:
                                                        MainAxisAlignment.start,
                                                    crossAxisAlignment:
                                                        CrossAxisAlignment
                                                            .start,
                                                    children: <Widget>[
                                                      Flexible(
                                                        child: Container(
                                                            height: 50.0,
                                                            child: Text(
                                                              "Education",
                                                              maxLines: 1,
//                                                    overflow: TextOverflow.ellipsis,
                                                              style: TextStyle(
                                                                  fontSize:
                                                                      20.0,
                                                                  color: MyColors
                                                                      .bkColor,
                                                                  fontWeight:
                                                                      FontWeight
                                                                          .bold),
                                                            )),
                                                      ),
                                                    ],
                                                  ),
//
                                                ],
                                              ),
                                            ),
                                          ),
                                        ),
                                      ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text(
                                                "Current / Ongoing course",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          SelectSemister(
//                                                              coid: coid,
                                                            id: _id,
                                                            studentId: widget
                                                                .studentId,
//                                                              collegename:
//                                                                  collegename,
//                                                              university:
//                                                                  _university,
//                                                              degree: _degree,
//                                                              branch: _branch,
//                                                              startYear:
//                                                                  startYear,
//                                                              endYear: endYear,
//                                                              aggregateper:
//                                                                  _aggregateper,
//                                                              straggregatePercentage:
//                                                                  _straggregatePercentage,
//                                                              semister:
//                                                                  _semisternumber,
//                                                              educationgapyear:
//                                                                  educationgapyear,
//                                                              lateralentry:
//                                                                  lateralentry,

//                                                              educationGap:
//                                                                  educationGap
//                                                                      .toString()
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),



                                      Container(
                                        margin: const EdgeInsets.only(
                                            top: 14, bottom: 0,),
                                        child: RaisedButton(


                                            color: Colors.white,
                                            padding: EdgeInsets.symmetric(
                                                horizontal: 10.0, vertical: 2.0),
                                            shape: RoundedRectangleBorder(
                                                side: BorderSide(
                                                    color: Colors.white,
                                                    width: 1,
                                                    style: BorderStyle.solid),
                                                borderRadius:
                                                BorderRadius.circular(10)),
                                            child: Padding(
                                              padding: const EdgeInsets.all(0.0),

                                                  child: Row(
                                                    // Replace with a Row for horizontal icon + text
                                                    children: <Widget>[
                                                      Padding(
                                                        padding:
                                                        const EdgeInsets.all(
                                                            8.0),
                                                        child: Image.asset(
                                                            'assets/images/cap_icon.png',
                                                            width: 30.0,
                                                            height: 30.0),
                                                      ),
                                                      _degree == "null"
                                                          ? Text(
                                                        "-",
                                                        maxLines: 2,
                                                        style: TextStyle(
                                                            color: Colors
                                                                .black),
                                                      )
                                                          : Text(
                                                        _degree,
                                                        maxLines: 2,

                                                        style: TextStyle( fontSize: 12.0,
                                                            color: Colors
                                                                .black,fontWeight: FontWeight.bold,),
                                                      ),
                                                    ],
                                                  ),

                                            ),


                                            onPressed: () {
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                      // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                      SelectSemister(
//                                                          coid: coid,
                                                        id: _id,
                                                        studentId:
                                                        widget.studentId,
//                                                          collegename:
//                                                              collegename,
//                                                          university:
//                                                              _university,
//                                                          degree: _degree,
//                                                          branch: _branch,
//                                                          startYear: startYear,
//                                                          endYear: endYear,
//                                                          aggregateper:
//                                                              _aggregateper,
//                                                          straggregatePercentage:
//                                                              _straggregatePercentage,
//                                                          semister:
//                                                              _semisternumber,
//                                                          educationgapyear:
//                                                              educationgapyear,
//                                                          lateralentry:
//                                                              lateralentry,
//                                                          educationGap:
//                                                              educationGap
//                                                                  .toString()
                                                      )));
                                            }),
                                      ),


















                                      GestureDetector(
                                        onTap: () {
                                          Navigator.push(
                                              context,
                                              MaterialPageRoute(
                                                  builder: (context) =>
                                                      // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                      SelectSemister(
//                                                          coid: coid,
                                                        id: _id,
                                                        studentId:
                                                            widget.studentId,
//                                                          collegename:
//                                                              collegename,
//                                                          university:
//                                                              _university,
//                                                          degree: _degree,
//                                                          branch: _branch,
//                                                          startYear: startYear,
//                                                          endYear: endYear,
//                                                          aggregateper:
//                                                              _aggregateper,
//                                                          straggregatePercentage:
//                                                              _straggregatePercentage,
//                                                          semister:
//                                                              _semisternumber,
//                                                          educationgapyear:
//                                                              educationgapyear,
//                                                          lateralentry:
//                                                              lateralentry,
//                                                          educationGap:
//                                                              educationGap
//                                                                  .toString()
                                                      )));
                                        },
                                        child: Container(
                                          child: Card(
                                            color: Colors.white,
                                            elevation: 5.0,
                                            shape: RoundedRectangleBorder(
                                                borderRadius:
                                                    BorderRadius.circular(
                                                        15.0)),
                                            child: InkWell(
                                              child: Padding(
                                                padding: const EdgeInsets.only(
                                                    left: 10.0,
                                                    top: 4.0,
                                                    right: 10.0,
                                                    bottom: 10.0),
                                                child: Column(
                                                  children: <Widget>[
                                                    SizedBox(height: 00.0),
//                                                     Row(
//                                                       children: <Widget>[
//                                                         Image.asset(
//                                                             'assets/images/cap_icon.png',
//                                                             width: 40.0,
//                                                             height: 40.0),
//
//                                                         _degree == "null"
//                                                             ? Text(
//                                                                 "-",
//                                                                 maxLines: 2,
//                                                                 style: TextStyle(
//                                                                     color: Colors
//                                                                         .black),
//                                                               )
//                                                             : Text(
//                                                                 _degree,
//                                                                 maxLines: 2,
//                                                                 style: TextStyle(
//                                                                     color: Colors
//                                                                         .black),
//                                                               ),
//                                                         Expanded(
//                                                           child: Container(),
//                                                         ),
// //                                            Image.asset(
// //                                                'assets/images/icon_green_mark.jpg',
// //                                                width: 40.0,
// //                                                height: 40.0),
//                                                       ],
//                                                     ),
                                                    Row(
                                                      children: <Widget>[
                                                        Padding(
                                                          padding: const EdgeInsets.only(top:8.0,left: 8),
                                                          child: Icon(Icons.library_books,
                                                            color: MyColors.bkColor,size: 20,),),

                                                        Padding(
                                                          padding: const EdgeInsets.only(top:15.0,left:10),
                                                          child: _branch == "null"
                                                              ? Text(
                                                                  "-",
                                                                  maxLines: 2,
                                                                  style: TextStyle(
                                                                      color: Colors
                                                                          .black),
                                                                )
                                                              : Text(
                                                                  _branch,
                                                                  maxLines: 2,
                                                                  style: TextStyle(
                                                                      color: Colors
                                                                          .black,fontWeight: FontWeight.bold),
                                                                ),
                                                        ),
                                                        Expanded(
                                                          child: Container(),
                                                        ),
//                                            Image.asset(
//                                                'assets/images/icon_green_mark.jpg',
//                                                width: 40.0,
//                                                height: 40.0),
                                                      ],
                                                    ),
                                                    Padding(
                                                      padding:
                                                          const EdgeInsets.only(
                                                              top: 0,
                                                              left: 0),
                                                      child: Row(
                                                        children: <Widget>[
                                                          Padding(
                                                            padding: const EdgeInsets.all(8.0),
                                                            child: Icon(Icons.edit,
                                                              color: MyColors.bkColor,size: 20,),
                                                          ),
                                                          Text(
                                                            _semister,
                                                            style: TextStyle(
                                                                color: Colors
                                                                    .green,fontWeight: FontWeight.bold),
                                                          ),
                                                          Text(
                                                            " - Semester",
                                                            style: TextStyle(
                                                                color: Colors
                                                                    .grey,fontWeight: FontWeight.bold),
                                                          ),
                                                          Expanded(
                                                            child: Container(),
                                                          ),


                                                         Text(
                                                                  _aggregateper,
                                                                  style: TextStyle(
                                                                      color: Colors
                                                                          .green,fontWeight: FontWeight.bold),
                                                                ),
                                                         Text( " - CGPA",
                                                                  style: TextStyle(
                                                                      color: Colors
                                                                          .grey,fontWeight: FontWeight.bold),
                                                                ),
                                                        ],
                                                      ),
                                                    ),
                                                    Padding(
                                                      padding:
                                                          const EdgeInsets.only(
                                                              bottom: 10),
                                                      child: Row(
                                                        children: <Widget>[
                                                          Padding(
                                                            padding: const EdgeInsets.all(8.0),
                                                            child: Icon(
                                                              Icons.date_range,
                                                              color:
                                                                 MyColors.bkColor,
                                                              size: 20,),
                                                          ),
                                                          // _strduration == "null"
                                                          //     ? Text(
                                                          //         "-",
                                                          //         style: TextStyle(
                                                          //             color: Colors
                                                          //                 .black),
                                                          //       )
                                                          //     :
                                                          Text(
                                                                  _strduration,
                                                                  style: TextStyle(
                                                                      color: Colors.green
                                                                          ,fontWeight: FontWeight.bold),
                                                                ),
                                                          Expanded(
                                                            child: Container(),
                                                          ),

                                                          Text(
                                                                  _straggregatePercentage ,
                                                                  style: TextStyle(
                                                                      color: Colors
                                                                          .green,fontWeight: FontWeight.bold),
                                                                ),
                                                          Text( " - Aggregate%",
                                                                  style: TextStyle(
                                                                      color: Colors
                                                                          .grey,fontWeight: FontWeight.bold),
                                                                ),
                                                        ],
                                                      ),
                                                    ),
                                                  ],
                                                ),
                                              ),
                                            ),
                                          ),
                                        ),
                                      ),

//                            Divider(
//                              color: Colors.black38,
//                            ),

//
//                            Divider(
//                              color: Colors.black38,
//                            ),
//
                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Semester wise scores",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),


                                          FlatButton(
                                            onPressed: () {


                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          Semestarnumber(
                                                            studentId: widget
                                                                .studentId,
                                                            semister:
                                                                _semisternumber,
                                                            lateralentry:
                                                                lateralentry,
                                                            sem1: _sem1,
                                                            sem2: _sem2,
                                                            sem3: _sem3,
                                                            sem4: _sem4,
                                                            sem5: _sem5,
                                                            sem6: _sem6,
                                                            sem7: _sem7,
                                                            sem8: _sem8,
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      semisterList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add semester details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 170,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: DriveHighlights(context),
                                            ),





                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Previous education",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddEducation(
                                                            id: "",
                                                            studentId: widget
                                                                .studentId,
                                                            degree: "",
                                                            educationcenter: "",
                                                            educationboard: "",
                                                            percentage: "",
                                                            cgpa: "",
                                                            startyear: "",
                                                            endyear: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),



//





                                      prevEducationList.isEmpty
                                          ? Container(
                                              height: 160,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add education details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 200,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: PreviousEducation(context),
                                            ),

//                                  Container(
//                                    margin: const EdgeInsets.only(
//                                        top: 14, bottom: 6),
//                                    child: GestureDetector(
////                                      onTap:(){ lodePdf();},
//                                      child: Card(
//                                        elevation: 5.0,
//                                        color: Colors.red[300],
//                                        shape: RoundedRectangleBorder(
//                                            borderRadius:
//                                                BorderRadius.circular(50.0)),
//                                        child: Padding(
//                                          padding: const EdgeInsets.all(8.0),
//                                          child: Text("View Resume",
//                                              textAlign: TextAlign.center,
//                                              style: TextStyle(
//                                                fontSize: 12.0,
//                                                color: Colors.white,
//                                              )),
//                                        ),
//                                      ),
//                                    ),
//                                  ),










                                      Row(
                                        children: <Widget>[

                                          Column(
                                            children: <Widget>[

                                              Padding(
                                                padding: const EdgeInsets.all(8.0),
                                                child: Text("Upload resume",
                                                    textAlign: TextAlign.center,
                                                    style: TextStyle(
                                                        fontSize: 16.0,
                                                        color: Colors.black,
                                                        fontWeight:
                                                        FontWeight.bold)),
                                              ),

                                              Padding(
                                                padding: const EdgeInsets.all(0.0),
                                                child: Text("*Upto 300 kb only",
                                                    textAlign: TextAlign.center,
                                                    style: TextStyle(
                                                        fontSize: 12.0,
                                                        color: Colors.black,)),
                                              ),

                                            ],
                                          ),


                                          Expanded(
                                            child: Container(),
                                          ),


                                        ],
                                      ),










                                      _resume=="null"?

                                      Container(
                                        margin: const EdgeInsets.only(
                                          top: 14, bottom: 0,),
                                        child: RaisedButton(


                                            color: Colors.white,
                                            padding: EdgeInsets.symmetric(
                                                horizontal: 10.0, vertical: 2.0),
                                            shape: RoundedRectangleBorder(
                                                side: BorderSide(
                                                    color: Colors.white,
                                                    width: 1,
                                                    style: BorderStyle.solid),
                                                borderRadius:
                                                BorderRadius.circular(10)),
                                            child: Padding(
                                              padding: const EdgeInsets.all(0.0),

                                              child: Row(
                                                // Replace with a Row for horizontal icon + text
                                                children: <Widget>[
                                                  Padding(
                                                      padding:
                                                      const EdgeInsets.all(
                                                          8.0),
                                                      child:Icon( Icons.insert_drive_file,color: MyColors.bkColor,)

                                                  ),
                                                  Padding(
                                                    padding:
                                                    const EdgeInsets.only(left:20,top:8,right: 8.0,bottom: 8),

                                                    child: Text(
                                                      "Upload pdf file",
                                                      style: TextStyle( fontSize: 14.0,
                                                        color: MyColors.bkColor,fontWeight: FontWeight.bold,),
                                                    ),),

                                                  Expanded(
                                                    child: Container(),
                                                  ),

                                                   Padding(
                                                      padding:
                                                      const EdgeInsets.all(
                                                          8.0),
                                                    // child: Image.asset(
                                                    //     'assets/images/icon_green_mark.jpg',
                                                    //     width: 30.0,
                                                    //     height: 30.0),

                                                  ),
                                                ],
                                              ),

                                            ),


                                            onPressed: () {
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                    builder: (context) =>
                                                    // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                    Uploadresume(
                                                        studentId: widget.studentId,
                                                        resume:_resume),));
                                            }),
                                      )  :        Container(
                                        margin: const EdgeInsets.only(
                                          top: 14, bottom: 0,),
                                        child: RaisedButton(


                                            color: Colors.white,
                                            padding: EdgeInsets.symmetric(
                                                horizontal: 10.0, vertical: 2.0),
                                            shape: RoundedRectangleBorder(
                                                side: BorderSide(
                                                    color: Colors.white,
                                                    width: 1,
                                                    style: BorderStyle.solid),
                                                borderRadius:
                                                BorderRadius.circular(10)),
                                            child: Padding(
                                              padding: const EdgeInsets.all(0.0),

                                              child: Row(
                                                // Replace with a Row for horizontal icon + text
                                                children: <Widget>[
                                                  Padding(
                                                      padding:
                                                      const EdgeInsets.all(
                                                          8.0),
                                                      child:Icon( Icons.insert_drive_file,color: MyColors.bkColor,)

                                                  ),
                                                  Padding(
                                                    padding:
                                                    const EdgeInsets.only(left:20,top:8,right: 8.0,bottom: 8),

                                                    child: Text(
                                                      "Update resume",
                                                      style: TextStyle( fontSize: 14.0,
                                                        color: MyColors.bkColor,fontWeight: FontWeight.bold,),
                                                    ),),

                                                  Expanded(
                                                    child: Container(),
                                                  ),


                                                  Padding(
                                                    padding:
                                                    const EdgeInsets.all(
                                                        8.0),
                                                    child: Image.asset(
                                                        'assets/images/icon_green_mark.jpg',
                                                        width: 30.0,
                                                        height: 30.0),

                                                  )
                                                ],
                                              ),

                                            ),


                                            onPressed: () {
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                    builder: (context) =>
                                                    // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                    Uploadresume(
                                                        studentId: widget.studentId,
                                                        resume:_resume),));
                                            }),
                                      ),


                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Career objective",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          co.isEmpty
                                              ? FlatButton(
                                                  onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                                    Navigator.push(
                                                        context,
                                                        MaterialPageRoute(
                                                            builder:
                                                                (context) =>
                                                                    // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                                    AddCareerObjective(
                                                                        strCareerObjective:
                                                                            co,
                                                                        strcoid:
                                                                            studentObjectiveid,
                                                                        studentid:
                                                                            widget.studentId)));
                                                  },
                                                  child: new Icon(
                                                      Icons.add_circle,
                                                      color: MyColors.bkColor),
                                                )
                                              : FlatButton(
                                                  onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                                    Navigator.push(
                                                        context,
                                                        MaterialPageRoute(
                                                            builder:
                                                                (context) =>
                                                                    // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                                    AddCareerObjective(
                                                                        strCareerObjective:
                                                                            co,
                                                                        strcoid:
                                                                            studentObjectiveid,
                                                                        studentid:
                                                                            widget.studentId)));
                                                  },
                                                  child: new Icon(Icons.edit,
                                                      color: MyColors.bkColor),
                                                )
                                        ],
                                      ),

                                      co.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add career objective",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              width: double.infinity,
//                                w: 170,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Careerobjective(context),
                                            ),
//

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Internships & work",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddInternship(
                                                            id: "",
                                                            studentId: widget
                                                                .studentId,
                                                            title: "",
                                                            company: "",
                                                            location: "",
                                                            start: "",
                                                            end: "",
                                                            salary: "",
                                                            description: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentInternshipList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add internship details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Internships_workexperience(
                                                  context),
                                            ),

                                      ////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Technical skills",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          TechnicalSkills(
                                                              id: "",
                                                              studentId: widget
                                                                  .studentId,
                                                              skill: "",
                                                              proficiency:
                                                                  "")));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentSkillsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add skills details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 170,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Technicalskills(context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Projects",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddProjects(
                                                            id: "",
                                                            studentId: widget
                                                                .studentId,
                                                            title: "",
                                                            skill: "",
                                                            start: "",
                                                            end: "",
                                                            college: "",
                                                            description: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),
                                      studentProjectsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add projects details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 170,
//                                width: 350,
//                              width: double.infinity,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Projects(context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Languages",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          Language(
                                                              id: "",
                                                              studentId: widget
                                                                  .studentId,
                                                              lang: "",
                                                              proficiency:
                                                                  "")));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentLanguagesList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add languages details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Communicationlanguages(
                                                  context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text(
                                                "Awards and recognitions",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddAwards(
                                                            id: "",
                                                            studentId: widget
                                                                .studentId,
                                                            title: "",
                                                            issuer: "",
                                                            issueyear: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentAwardsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add awards details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 100,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child:
                                                  Awards_recognitions(context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Certifications",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddCertifications(
                                                            id: "",
                                                            studentId: widget
                                                                .studentId,
                                                            name: "",
                                                            authority: "",
                                                            url: "",
                                                            licence: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentCertificationsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add certifications details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 100,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Certifications(context),
                                            ),

//
                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Competitions",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddCompetition(
                                                            studentId: widget
                                                                .studentId,
                                                            id: "",
                                                            title: "",
                                                            position: "",
                                                            year: "",
                                                            college: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentCompetitionsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add competitions details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 100,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Competitions(context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text(
                                                "Conferences & workshops",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddConferences(
                                                            title: "",
                                                            organizer: "",
                                                            year: "",
                                                            address: "",
                                                            description: "",
                                                            studentId: widget
                                                                .studentId,
                                                            id: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),
                                      studentConferencesList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add conferences details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 100,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Conferences_workshops(
                                                  context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Test scores",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddTestScore(
                                                            id: "",
                                                            studentId: widget
                                                                .studentId,
                                                            title: "",
                                                            year: "",
                                                            obtain: "",
                                                            total: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentTestsList.isEmpty
                                          ? Container(
                                              height: 160,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add test details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 170,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Test_scores(context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Patents",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddPatent(
                                                            id: "",
                                                            studentId: widget
                                                                .studentId,
                                                            title: "",
                                                            office: "",
                                                            app_number: "",
                                                            status: "",
                                                            date: "",
                                                            description: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentPatentsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add patents details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 100,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Patents(context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Publications",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddPublication(
                                                            studentId: widget
                                                                .studentId,
                                                            id: "",
                                                            title: "",
                                                            publisher: "",
                                                            url: "",
                                                            year: "",
                                                            description: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentPublicationsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add publications details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 100,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Publications(context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Scholarships",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddScholarships(
                                                            studentId: widget
                                                                .studentId,
                                                            id: "",
                                                            title: "",
                                                            year: "",
                                                            college: "",
                                                            description: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentScholarshipsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add scholarships details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 100,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child: Scholarships(context),
                                            ),

                                      Row(
                                        children: <Widget>[
                                          Padding(
                                            padding: const EdgeInsets.all(8.0),
                                            child: Text("Curriculum activity",
                                                textAlign: TextAlign.center,
                                                style: TextStyle(
                                                    fontSize: 16.0,
                                                    color: Colors.black,
                                                    fontWeight:
                                                        FontWeight.bold)),
                                          ),
                                          Expanded(
                                            child: Container(),
                                          ),
                                          FlatButton(
                                            onPressed: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
                                              Navigator.push(
                                                  context,
                                                  MaterialPageRoute(
                                                      builder: (context) =>
                                                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                                                          AddCurriculumActivity(
                                                            id: "",
                                                            studentId: widget
                                                                .studentId,
                                                            title: "",
                                                            description: "",
                                                          )));
                                            },
                                            child: new Icon(Icons.add_circle,
                                                color: MyColors.bkColor),
                                          )
                                        ],
                                      ),

                                      studentCurriculumsList.isEmpty
                                          ? Container(
                                              height: 130,
                                              margin: const EdgeInsets.only(
                                                  top: 40),
                                              child: Text(
                                                "Please add curriculum details",
                                                style: TextStyle(
                                                    color: Colors.black),
                                              ),
                                            )
                                          : Container(
                                              height: 100,
                                              margin: const EdgeInsets.only(
                                                  bottom: 10, top: 16),
                                              child:
                                                  Curriculumactivity(context),
                                            ),

//
//                            Divider(
//                              color: Colors.black38,
//                            ),
//
//                            DriveDetails(_details),
//                            Divider(
//                              color: Colors.black38,
//                            ),
//
//                            CompanyDetails(_details),
//                            Divider(
//                              color: Colors.black38,
//                            ),
//
//                            _applyButton()
                                    ],
                                  ),
                                )
                              ],
                            ),
                          ],
                        ),
                      ),
                    ),
                  )));
  }

  Future<Null> _refresh() async {
    await new Future.delayed(new Duration(seconds: 3));

    setState(() {
      semisterList.clear();
      prevEducationList.clear();

      studentSkillsList.clear();
      studentLanguagesList.clear();

      studentTestsList.clear();
      studentPatentsList.clear();

      studentProjectsList.clear();
      studentAwardsList.clear();

      studentCertificationsList.clear();
      studentConferencesList.clear();
      studentCompetitionsList.clear();

      studentPublicationsList.clear();
      studentScholarshipsList.clear();
      studentCurriculumsList.clear();

      studentInternshipList.clear();

      createEduDetails(widget.studentId);

      createVerifi(widget.studentId);
    });

    return null;
  }

  Widget Internships_workexperience(BuildContext context) {
    final titles = [
      'Semister 1',
      'Semister 2',
      'Semister 3',
      'Semister 4',
      'Semister 5',
      'Semister 6',
    ];

    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentInternshipList.length,
      itemBuilder: (context, index) {
        String id = studentInternshipList[index].id;
        String studentId = studentInternshipList[index].studentId;
        String title = studentInternshipList[index].title;
        String company = studentInternshipList[index].company;
        String location = studentInternshipList[index].location;
        String start = studentInternshipList[index].start;
        String end = studentInternshipList[index].end;
        String salary = studentInternshipList[index].salary;
        String description = studentInternshipList[index].description;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          StudentInternship(
                            id: id,
                            studentId: studentId,
                            title: title,
                            company: company,
                            location: location,
                            start: start,
                            end: end,
                            salary: salary,
                            description: description,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset('assets/images/icon_internship.png',
                            width: 40.0, height: 40.0),
                      ),

                      Expanded(child: Text(studentInternshipList[index].title)),

                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),

//                            onPressed: awarddelete),
                      ),

//
//                      Text(studentCurriculumsList[index].title, maxLines: 2,),
                    ],
                  ),
                ),
//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  Widget DriveHighlights(BuildContext context) {
    return ListView.builder(
      scrollDirection: Axis.horizontal,
//            children: semisterList,

      itemCount: semisterList.length,

      itemBuilder: (context, index) {
        String strid = semisterList[index].id;
        String strstudentId = semisterList[index].studentId;
        String strsemester = semisterList[index].semester;
        String strsgpa = semisterList[index].sgpa;
        String strpercentage = semisterList[index].percentage;
        String strcurr_session = semisterList[index].curr_session;
        String strbacklog = semisterList[index].backlog;
        String strongoingbacklogs = semisterList[index].ongoingbacklogs;

        return GestureDetector(
          //You need to make my child interactive
          onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
            Navigator.push(
                context,
                MaterialPageRoute(
                    builder: (context) =>
                        // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                        SemistarDetails(
                          strcollegename: collegename,
                          strid: strid,
                          strstudentId: strstudentId,
                          strsemester: strsemester,
                          strsgpa: strsgpa,
                          strpercentage: strpercentage,
                          strcurr_session: strcurr_session,
                          strbacklog: strbacklog,
                          strongoingbacklogs: strongoingbacklogs,
                        )));
          },
          child: new Padding(
            padding: const EdgeInsets.all(8.0),
            child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                    width: 140,
                    child: Column(children: <Widget>[
                      Row(
                        mainAxisAlignment: MainAxisAlignment.end,
                        crossAxisAlignment: CrossAxisAlignment.end,
                        children: <Widget>[
                          Flexible(
                            child: Container(
                              height: 20.0,
//                                child: Image.asset(
//                                    'assets/images/icon_green_mark.jpg',
//                                    width: 20.0,
//                                    height: 20.0),
                            ),
                          ),
                        ],
                      ),
                      Image.asset('assets/images/cap_icon.png',
                          width: 50.0, height: 50.0),
                      Container(
                        margin: const EdgeInsets.only(
                          top: 6,
                        ),
                        child: Text("Semester " + semisterList[index].semester),
                      ),
                      Container(
                        margin: const EdgeInsets.only(top: 14, bottom: 6),
                        child: Card(
                          color: Colors.deepPurple[200],
                          shape: RoundedRectangleBorder(
                              borderRadius: BorderRadius.circular(50.0)),
                          child: Padding(
                            padding: const EdgeInsets.all(8.0),
                            child: Text(
                                "PERCENT " + semisterList[index].percentage,
                                textAlign: TextAlign.center,
                                style: TextStyle(
                                    fontSize: 12.0,
                                    color: MyColors.bkColor,
                                    fontWeight: FontWeight.bold)),
                          ),
                        ),
                      )
                    ]))

//             Text(titles[index]),
                ),
          ),
        );
      },
    );
  }

  Widget Technicalskills(BuildContext context) {
    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentSkillsList.length,
      itemBuilder: (context, index) {
        String id = studentSkillsList[index].id;
        String proficiency = studentSkillsList[index].proficiency;
        String skill = studentSkillsList[index].skill;
        return Padding(
            padding: const EdgeInsets.all(8.0),
            child: GestureDetector(
              onTap: () {
                var route = MaterialPageRoute(
                  builder: (BuildContext context) => new TechnicalSkills(
                      id: id,
                      studentId: widget.studentId,
                      skill: skill,
                      proficiency: proficiency),
                );
                Navigator.of(context).push(route);
              },
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  margin: const EdgeInsets.all(5),
                  width: 300,
                  child: Column(
                    children: <Widget>[
                      Row(
                        children: <Widget>[
                          Padding(
                            padding: const EdgeInsets.only(right: 20, left: 20),
                            child: Image.asset(
                                'assets/images/icon_technical_skill.png',
                                width: 40.0,
                                height: 40.0),
                          ),
                          Column(children: <Widget>[
                            Container(
                              margin: const EdgeInsets.only(
                                  top: 14, bottom: 6, left: 10),
                              child: Center(
                                child: Card(
                                  color: Colors.green[300],
                                  shape: RoundedRectangleBorder(
                                      borderRadius:
                                          BorderRadius.circular(50.0)),
                                  child: Padding(
                                    padding: const EdgeInsets.all(8.0),
                                    child: Text(studentSkillsList[index].skill,
                                        textAlign: TextAlign.center,
                                        style: TextStyle(
                                          fontSize: 12.0,
                                          color: Colors.white,
                                        )),
                                  ),
                                ),
                              ),
                            ),
                            Container(
                              margin: const EdgeInsets.only(
                                  top: 3, bottom: 6, left: 0),
                              child: Center(
                                child:
                                    Text(studentSkillsList[index].proficiency,
                                        textAlign: TextAlign.center,
                                        style: TextStyle(
                                          fontSize: 10.0,
                                          color: Colors.grey[400],
                                        )),
                              ),
                            ),
                          ]),
                          Container(
                            margin: const EdgeInsets.only(
                                top: 14, bottom: 10, right: 10),

//                            onPressed: awarddelete),
                          ),
                        ],
                      ),
                    ],
                  ),

//             Text(titles[index]),
                ),
              ),
            ));
      },
    );
  }

  Widget Communicationlanguages(BuildContext context) {
    final titles = [
      'Marathi',
      'English',
      'Hindi',
    ];

    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentLanguagesList.length,
      itemBuilder: (context, index) {
        String id = studentLanguagesList[index].id;
        String language = studentLanguagesList[index].language;
        String proficiency = studentLanguagesList[index].proficiency;

        return Padding(
            padding: const EdgeInsets.all(8.0),
            child: GestureDetector(
              onTap: () {
                var route = MaterialPageRoute(
                  builder: (BuildContext context) => new Language(
                      id: id,
                      studentId: widget.studentId,
                      lang: language,
                      proficiency: proficiency),
                );
                Navigator.of(context).push(route);
              },
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  margin: const EdgeInsets.all(5),
                  width: 270,
                  child: Column(
                    children: <Widget>[
                      Row(
                        children: <Widget>[
                          Center(
                            child: Padding(
                              padding:
                                  const EdgeInsets.only(right: 10, left: 6),
                              child: Image.asset(
                                  'assets/images/icon_language.png',
                                  width: 40.0,
                                  height: 40.0),
                            ),
                          ),
                          Center(
                            child: Column(
                              children: <Widget>[
                                Container(
                                  margin: const EdgeInsets.only(
                                      top: 14, bottom: 6, left: 20),
                                  child: Center(
                                    child: Card(
                                      color: Colors.green[300],
                                      shape: RoundedRectangleBorder(
                                          borderRadius:
                                              BorderRadius.circular(50.0)),
                                      child: Padding(
                                        padding: const EdgeInsets.all(8.0),
                                        child: Text(
                                            studentLanguagesList[index]
                                                .language,
                                            textAlign: TextAlign.center,
                                            style: TextStyle(
                                              fontSize: 12.0,
                                              color: Colors.white,
                                            )),
                                      ),
                                    ),
                                  ),
                                ),
                                Container(
                                  margin: const EdgeInsets.only(
                                      top: 3, bottom: 6, left: 20),
                                  child: Text(
                                      studentLanguagesList[index].proficiency,
                                      textAlign: TextAlign.center,
                                      style: TextStyle(
                                        fontSize: 10.0,
                                        color: Colors.grey[400],
                                      )),
                                )
                              ],
                            ),
                          ),
                          Container(
                            margin: const EdgeInsets.only(
                                top: 14, bottom: 10, right: 10),

//                            onPressed: awarddelete),
                          ),
                        ],
                      ),
                    ],
                  ),

//             Text(titles[index]),
                ),
              ),
            ));
      },
    );
  }

  Widget PreviousEducation(BuildContext context) {
    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: prevEducationList.length,
      itemBuilder: (context, index) {
        String id = prevEducationList[index].id;
        String studentId = prevEducationList[index].studentId;
        String degree = prevEducationList[index].degree;
        String educationcenter = prevEducationList[index].educationcenter;
        String educationboard = prevEducationList[index].educationboard;
        String grade = prevEducationList[index].grade;
        String percentage = prevEducationList[index].percentage;
        String cgpa = prevEducationList[index].cgpa;
        String startyear = prevEducationList[index].startyear;
        String endyear = prevEducationList[index].endyear;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoPreviousEducation(
                            strid: id,
                            strstudentId: studentId,
                            strdegree: degree,
                            streducationcenter: educationcenter,
                            streducationboard: educationboard,
                            strgrade: grade,
                            strpercentage: percentage,
                            strcgpa: cgpa,
                            strstartyear: startyear,
                            strendyear: endyear,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                  shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(10.0)),
                  elevation: 5.0,
                  margin: const EdgeInsets.symmetric(horizontal: 3.0),
                  child: Container(
                      width: 140,
                      child: Column(children: <Widget>[
                        Row(
                          mainAxisAlignment: MainAxisAlignment.end,
                          crossAxisAlignment: CrossAxisAlignment.end,
                          children: <Widget>[
                            Flexible(
                              child: Container(
                                height: 20.0,
//                            child: Image.asset(
//                                'assets/images/icon_green_mark.jpg',
//                                width: 20.0,
//                                height: 20.0),
                              ),
                            ),
                          ],
                        ),
                        Image.asset('assets/images/icon_previous_education.png',
                            width: 50.0, height: 50.0),
                        Container(
                          margin: const EdgeInsets.only(
                            top: 4,
                          ),
                          child: Text(prevEducationList[index].degree),
                        ),
                        Container(
                          margin: const EdgeInsets.only(top: 14, bottom: 6),
                          child: Card(
                            color: Colors.deepPurple[300],
                            shape: RoundedRectangleBorder(
                                borderRadius: BorderRadius.circular(50.0)),
                            child: Padding(
                              padding: const EdgeInsets.all(8.0),
                              child: Text(
                                  prevEducationList[index].percentage +
                                      " PERCENT",
                                  textAlign: TextAlign.center,
                                  style: TextStyle(
                                      fontSize: 12.0,
                                      color: MyColors.bkColor,
                                      fontWeight: FontWeight.bold)),
                            ),
                          ),
                        ),
                      ]))

//             Text(titles[index]),
                  ),
            ));
      },
    );
  }

  Widget Careerobjective(BuildContext context) {
    return Card(
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(10.0)),
      elevation: 5.0,
      child: Column(
        children: <Widget>[
          '${studentObjective['objective']}' == null
              ? Padding(
                  padding: const EdgeInsets.only(
                      left: 12.0, right: 12, bottom: 12, top: 8),
                  child: Container(
//                  height: 50.0,
                      child: Text(
                    "No data found",
//                                                    overflow: TextOverflow.ellipsis,
                    style: TextStyle(
//                        fontSize: 6.0,
                      color: Colors.black,
                    ),
//                        fontWeight:
//                        FontWeight.bold),
                  )),
                )
              : Padding(
                  padding: const EdgeInsets.only(
                      left: 12.0, right: 12, bottom: 12, top: 8),
                  child: Container(
//                  height: 50.0,
                      child: Text(
                    '${studentObjective['objective']}',
//                                                    overflow: TextOverflow.ellipsis,
                    style: TextStyle(
//                        fontSize: 6.0,
                      color: Colors.black,
                    ),
//                        fontWeight:
//                        FontWeight.bold),
                  )),
                ),

//
        ],
      ),
    );
  }

  Widget Projects(BuildContext context) {
    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentProjectsList.length,
      itemBuilder: (context, index) {
        String id = studentProjectsList[index].id;
        String studentId = studentProjectsList[index].studentId;
        String title = studentProjectsList[index].title;
        String skill = studentProjectsList[index].skill;
        String start = studentProjectsList[index].start;
        String end = studentProjectsList[index].end;
        String college = studentProjectsList[index].college;
        String description = studentProjectsList[index].description;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoProject(
                            strid: id,
                            strstudentId: studentId,
                            strtitle: title,
                            strskill: skill,
                            strstart: start,
                            strend: end,
                            strcollege: college,
                            strdescription: description,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                  shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(10.0)),
                  elevation: 5.0,
                  margin: const EdgeInsets.symmetric(horizontal: 3.0),
                  child: Container(
                      width: 300,
                      child: Column(children: <Widget>[
                        Image.asset('assets/images/icon_project.png',
                            width: 50.0, height: 50.0),
                        Padding(
                          padding: const EdgeInsets.all(8.0),
                          child: Text(
                            studentProjectsList[index].title,
                            maxLines: 2,
                            style: TextStyle(
                                color: Colors.black,
                                fontWeight: FontWeight.bold),
                          ),
                        ),
                        Padding(
                          padding: const EdgeInsets.all(8.0),
                          child: Container(
                              margin: const EdgeInsets.only(
                                top: 6,
                              ),
                              child: Text(
                                studentProjectsList[index].skill,
                                maxLines: 6,
                              )),
                        ),
                        Container(
                          margin: const EdgeInsets.only(
                              top: 14, bottom: 10, right: 10),

//                            onPressed: awarddelete),
                        ),
                      ]))

//             Text(titles[index]),
                  ),
            ));
      },
    );
  }

  ///////////////////////////////////////others data/////////////////////////////////////

  Widget Awards_recognitions(BuildContext context) {
    final titles = [
      'Social or Cultural Events',
      'Athletic Achievement',
      'Special Event',
      'Social or Cultural Events',
      'Athletic Achievement',
      'Special Event',
    ];

    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentAwardsList.length,
      itemBuilder: (context, index) {
        String id = studentAwardsList[index].id;
        String studentId = studentAwardsList[index].studentId;
        String title = studentAwardsList[index].title;
        String issuer = studentAwardsList[index].issuer;
        String issueyear = studentAwardsList[index].issueyear;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoStudentAwards(
                            strid: id,
                            strstudentId: studentId,
                            strtitle: title,
                            strissuer: issuer,
                            strissueyear: issueyear,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset('assets/images/icon_awards.png',
                            width: 40.0, height: 40.0),
                      ),
                      Expanded(
                          child: Text(studentAwardsList[index].title,
                              maxLines: 2)),
                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),
                      ),
                    ],
                  ),
                ),

//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  Widget Certifications(BuildContext context) {
    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentCertificationsList.length,
      itemBuilder: (context, index) {
        String id = studentCertificationsList[index].id;
        String studentId = studentCertificationsList[index].studentId;
        String name = studentCertificationsList[index].name;
        String authority = studentCertificationsList[index].authority;
        String url = studentCertificationsList[index].url;
        String licence = studentCertificationsList[index].licence;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoCertifications(
                            strid: id,
                            strstudentId: studentId,
                            strname: name,
                            strauthority: authority,
                            strurl: url,
                            strlicence: licence,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset(
                            'assets/images/icon_certification.png',
                            width: 40.0,
                            height: 40.0),
                      ),
                      Expanded(
                          child: Text(studentCertificationsList[index].name,
                              maxLines: 2)),
                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),

//                            onPressed: awarddelete),
                      ),
                    ],
                  ),
                ),

//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  Widget Competitions(BuildContext context) {
    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentCompetitionsList.length,
      itemBuilder: (context, index) {
        String id = studentCompetitionsList[index].id;
        String studentId = studentCompetitionsList[index].studentId;
        String title = studentCompetitionsList[index].title;
        String position = studentCompetitionsList[index].position;
        String year = studentCompetitionsList[index].year;
        String college = studentCompetitionsList[index].college;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoCompetitions(
                            id: id,
                            studentId: studentId,
                            title: title,
                            position: position,
                            year: year,
                            college: college,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset('assets/images/icon_competation.png',
                            width: 40.0, height: 40.0),
                      ),
                      Expanded(
                          child: Text(studentCompetitionsList[index].title,
                              maxLines: 2)),
                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),

//                            onPressed: awarddelete),
                      ),
                    ],
                  ),
                ),

//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  Widget Conferences_workshops(BuildContext context) {
    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentConferencesList.length,
      itemBuilder: (context, index) {
        String id = studentConferencesList[index].id;
        String studentId = studentConferencesList[index].studentId;
        String title = studentConferencesList[index].title;
        String organizer = studentConferencesList[index].organizer;
        String year = studentConferencesList[index].year;
        String address = studentConferencesList[index].address;
        String description = studentConferencesList[index].description;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoConferences_workshops(
                            id: id,
                            studentId: studentId,
                            title: title,
                            organizer: organizer,
                            year: year,
                            address: address,
                            description: description,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset(
                            'assets/images/icon_conferance_workshop.png',
                            width: 40.0,
                            height: 40.0),
                      ),
                      Expanded(
                          child: Text(studentConferencesList[index].title,
                              maxLines: 2)),
                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),

//                            onPressed: awarddelete),
                      ),
                    ],
                  ),
                ),

//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  Widget Test_scores(BuildContext context) {
    final titles = [
      'BITSAT',
      'GATE',
      'English',
      'BITSAT',
      'GATE',
      'English',
    ];

    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentTestsList.length,
      itemBuilder: (context, index) {
        String id = studentTestsList[index].id;
        String studentId = studentTestsList[index].studentId;
        String title = studentTestsList[index].title;
        String year = studentTestsList[index].year;
        String obtain = studentTestsList[index].obtain;
        String total = studentTestsList[index].total;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoTest_scores(
                            id: id,
                            studentId: studentId,
                            title: title,
                            year: year,
                            obtain: obtain,
                            total: total,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                  shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(10.0)),
                  elevation: 5.0,
                  margin: const EdgeInsets.symmetric(horizontal: 3.0),
                  child: Container(
                      margin: const EdgeInsets.only(
                        top: 6,
                      ),
                      width: 170,
                      child: Column(children: <Widget>[
                        Image.asset('assets/images/icon_test_score.png',
                            width: 50.0, height: 50.0),
                        Container(
                          margin: const EdgeInsets.only(
                            top: 6,
                          ),
                          child:
                              Text(studentTestsList[index].title, maxLines: 2),
                        ),
                        Container(
                          margin: const EdgeInsets.only(top: 14, bottom: 0),
                          child: Card(
                            color: Colors.green[300],
                            shape: RoundedRectangleBorder(
                                borderRadius: BorderRadius.circular(50.0)),
                            child: Padding(
                              padding: const EdgeInsets.all(8.0),
                              child: Text(studentTestsList[index].year,
                                  maxLines: 2,
                                  textAlign: TextAlign.center,
                                  style: TextStyle(
                                    fontSize: 12.0,
                                    color: Colors.white,
                                  )),
                            ),
                          ),
                        ),
                      ]))

//             Text(titles[index]),
                  ),
            ));
      },
    );
  }

  Widget Patents(BuildContext context) {
    final titles = [
      'patents 1',
      'patents 2',
      'patents 3',
      'patents 4',
      'patents 5',
    ];

    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentPatentsList.length,
      itemBuilder: (context, index) {
        String id = studentPatentsList[index].id;
        String studentId = studentPatentsList[index].studentId;
        String title = studentPatentsList[index].title;
        String office = studentPatentsList[index].office;
        String app_number = studentPatentsList[index].app_number;
        String status = studentPatentsList[index].status;
        String date = studentPatentsList[index].date;
        String description = studentPatentsList[index].description;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoPatents(
                            id: id,
                            studentId: studentId,
                            title: title,
                            office: office,
                            app_number: app_number,
                            status: status,
                            date: date,
                            description: description,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset('assets/images/icon_patent.png',
                            width: 40.0, height: 40.0),
                      ),
                      Expanded(
                          child: Text(
                        studentPatentsList[index].title,
                        maxLines: 2,
                        style: TextStyle(color: Colors.black),
                      )),
                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),
                      ),
                    ],
                  ),
                ),

//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  Widget Publications(BuildContext context) {
    final titles = [
      'Autobiography',
      'Bibliography',
      'Cartoon',
      'Chart',
      'Autobiography',
      'Bibliography',
      'Cartoon',
      'Chart',
    ];

    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentPublicationsList.length,
      itemBuilder: (context, index) {
        String id = studentPublicationsList[index].id;
        String studentId = studentPublicationsList[index].studentId;
        String title = studentPublicationsList[index].title;
        String publisher = studentPublicationsList[index].publisher;
        String url = studentPublicationsList[index].url;
        String year = studentPublicationsList[index].year;
        String description = studentPublicationsList[index].description;
        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoPublications(
                            id: id,
                            studentId: studentId,
                            title: title,
                            publisher: publisher,
                            url: url,
                            year: year,
                            description: description,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset('assets/images/icon_publication.png',
                            width: 40.0, height: 40.0),
                      ),
                      Expanded(
                          child: Text(
                        studentPublicationsList[index].title,
                        maxLines: 2,
                        style: TextStyle(color: Colors.black),
                      )),
                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),
                      ),
                    ],
                  ),
                ),

//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  Widget Scholarships(BuildContext context) {
    final titles = [
      'Scholarships 1',
      'Scholarships 2',
      'Scholarships 3',
      'Scholarships 4',
      'Scholarships 5',
      'Scholarships 6',
      'Scholarships 7',
    ];

    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentScholarshipsList.length,
      itemBuilder: (context, index) {
        String id = studentScholarshipsList[index].id;
        String studentId = studentScholarshipsList[index].studentId;
        String title = studentScholarshipsList[index].title;
        String year = studentScholarshipsList[index].year;
        String associated = studentScholarshipsList[index].associated;
        String description = studentScholarshipsList[index].description;

        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoScholarships(
                            id: id,
                            studentId: studentId,
                            title: title,
                            year: year,
                            associated: associated,
                            description: description,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset('assets/images/icon_scholarship.png',
                            width: 40.0, height: 40.0),
                      ),
                      Expanded(
                          child: Text(studentScholarshipsList[index].title,
                              maxLines: 2)),
                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),
                      ),
                    ],
                  ),
                ),

//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  Widget Curriculumactivity(BuildContext context) {
    return ListView.builder(
      scrollDirection: Axis.horizontal,
      itemCount: studentCurriculumsList.length,
      itemBuilder: (context, index) {
        String id = studentCurriculumsList[index].id;
        String studentId = studentCurriculumsList[index].studentId;
        String title = studentCurriculumsList[index].title;
        String description = studentCurriculumsList[index].description;
        return new GestureDetector(
            //You need to make my child interactive
            onTap: () {
//                    final snackBar = SnackBar(content: Text("Not Available"));
//                        Scaffold.of(context).showSnackBar(snackBar);
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) =>
                          // JobDetailsScreen(driveId: id,appliedText: 'APPLY')));
                          InfoCurriculumactivity(
                            id: id,
                            studentId: studentId,
                            title: title,
                            description: description,
                          )));
            },
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: Card(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                elevation: 5.0,
                margin: const EdgeInsets.symmetric(horizontal: 3.0),
                child: Container(
                  width: 300,
                  margin: EdgeInsets.only(top: 0.0, left: 10),
                  child: Row(
                    children: <Widget>[
                      Padding(
                        padding: const EdgeInsets.only(right: 20),
                        child: Image.asset(
                            'assets/images/icon_curriculam_activity.png',
                            width: 40.0,
                            height: 40.0),
                      ),

                      Expanded(
                          child: Text(studentCurriculumsList[index].title)),

                      Container(
                        margin: const EdgeInsets.only(
                            top: 14, bottom: 10, right: 10),
                      ),

//
//                      Text(studentCurriculumsList[index].title, maxLines: 2,),
                    ],
                  ),
                ),
//             Text(titles[index]),
              ),
            ));
      },
    );
  }

  _buildGoogleSignInButton() {
    if ('${user1['currenteducation']}' == "false") {
      showDialog(
            context: context,
            builder: (context) => new AlertDialog(
              shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(10.0)),
              title: new Text(''),
              content: new Text(
                'Please complete current education',
                style: TextStyle(
                    fontSize: 14.0,
                    fontWeight: FontWeight.w600,
                    color: MyColors.bkColor),
              ),
              actions: <Widget>[
                Container(
                  margin: EdgeInsets.all(10.0),
                  child: new GestureDetector(
                    onTap: () => Navigator.of(context).pop(false),
                    child: Text("Okay"),
                  ),
                ),
                SizedBox(height: 16),
              ],
            ),
          ) ??
          false;
    } else if ('${user1['preeducation']}' == "false") {
      showDialog(
            context: context,
            builder: (context) => new AlertDialog(
              shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(10.0)),
              title: new Text(''),
              content: new Text(
                'Please complete previous education*',
                style: TextStyle(
                    fontSize: 14.0,
                    fontWeight: FontWeight.w600,
                    color: MyColors.bkColor),
              ),
              actions: <Widget>[
                Container(
                  margin: EdgeInsets.all(10.0),
                  child: new GestureDetector(
                    onTap: () => Navigator.of(context).pop(false),
                    child: Text("Okay"),
                  ),
                ),
                SizedBox(height: 16),
              ],
            ),
          ) ??
          false;
    } else if ('${semstatus['semsterStatus']}' == "false") {
      showDialog(
            context: context,
            builder: (context) => new AlertDialog(
              shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(10.0)),
              title: new Text(''),
              content: new Text(
                'Please complete semester data*',
                style: TextStyle(
                    fontSize: 14.0,
                    fontWeight: FontWeight.w600,
                    color: MyColors.bkColor),
              ),
              actions: <Widget>[
                Container(
                  margin: EdgeInsets.all(10.0),
                  child: new GestureDetector(
                    onTap: () => Navigator.of(context).pop(false),
                    child: Text("Okay"),
                  ),
                ),
                SizedBox(height: 16),
              ],
            ),
          ) ??
          false;
    } else if ('${user1['persnaldetails']}' == "false") {
      showDialog(
            context: context,
            builder: (context) => new AlertDialog(
              shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(10.0)),
              title: new Text(''),
              content: new Text(
                'Please complete profile details',
                style: TextStyle(
                    fontSize: 14.0,
                    fontWeight: FontWeight.w600,
                    color: MyColors.bkColor),
              ),
              actions: <Widget>[
                Container(
                  margin: EdgeInsets.all(10.0),
                  child: new GestureDetector(
                    onTap: () => Navigator.of(context).pop(false),
                    child: Text("Okay"),
                  ),
                ),
                SizedBox(height: 16),
              ],
            ),
          ) ??
          false;
    } else {
      MobilePasswordChangeRequest(widget.studentId);
      pr.show();
    }
  }

  MobilePasswordChangeRequest(String studentId) async {
    final http.Response response = await http.post(
      'https://tnplive.com/mobi/sendVerificationRequest',
      headers: <String, String>{'Content-Type': 'application/json'},
      body: jsonEncode(<String, String>{
        "studentId": studentId,
      }),
    );

    if (response.statusCode == 200) {
      String responseString = response.body;

      print("the id is  ********** _CareerObjective" + responseString);

      Map LangData = new Map();

      LangData = json.decode(response.body);

      String strmessage = LangData['message'];

      pr.hide().whenComplete(() {
        _refresh();

        showDialog(
              context: context,
              builder: (context) => new AlertDialog(
                shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(10.0)),
                title: new Text(''),
                content: new Text(
                  "Student verification request sent successfully",
                  style: TextStyle(
                      fontSize: 16.0,
                      fontWeight: FontWeight.w600,
                      color: MyColors.bkColor),
                ),
                actions: <Widget>[
                  Container(
                    margin: EdgeInsets.all(10.0),
                    child: new GestureDetector(
                      onTap: () => Navigator.of(context).pop(false),
                      child: Text("Okay"),
                    ),
                  ),
                  SizedBox(height: 16),
                ],
              ),
            ) ??
            false;
      });
    }
  }
}

///////////////////////////////////////////////ADAPTER////////////////////////////////////////////////////////////

class SemisterList extends StatelessWidget {
  String id;
  String studentId;
  String semester;
  String sgpa;
  String percentage;
  String curr_session;
  String backlog;
  String ongoingbacklogs;

  SemisterList({
    Key key,
    this.id,
    this.studentId,
    this.semester,
    this.sgpa,
    this.percentage,
    this.curr_session,
    this.backlog,
    this.ongoingbacklogs,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class PrevEducationList extends StatelessWidget {
  String id;
  String studentId;
  String degree;
  String educationcenter;
  String educationboard;
  String grade;
  String percentage;
  String cgpa;
  String startyear;
  String endyear;

  PrevEducationList(
      {Key key,
      this.id,
      this.studentId,
      this.degree,
      this.educationcenter,
      this.educationboard,
      this.grade,
      this.percentage,
      this.cgpa,
      this.startyear,
      this.endyear})
      : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentSkillsList extends StatelessWidget {
  String id;
  String studentId;
  String skill;
  String proficiency;

  StudentSkillsList({
    Key key,
    this.id,
    this.studentId,
    this.skill,
    this.proficiency,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentProjectsList extends StatelessWidget {
  String id;
  String studentId;
  String title;
  String skill;
  String start;
  String end;
  String college;
  String description;

  StudentProjectsList(
      {Key key,
      this.id,
      this.studentId,
      this.title,
      this.skill,
      this.start,
      this.end,
      this.college,
      this.description})
      : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentLanguagesList extends StatelessWidget {
  String id;
  String studentId;
  String language;
  String proficiency;

  StudentLanguagesList({
    Key key,
    this.id,
    this.studentId,
    this.language,
    this.proficiency,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentAwardsList extends StatelessWidget {
  String id;
  String studentId;
  String title;
  String issuer;
  String issueyear;

  StudentAwardsList({
    Key key,
    this.id,
    this.studentId,
    this.title,
    this.issuer,
    this.issueyear,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentCertificationsList extends StatelessWidget {
  String id;
  String studentId;
  String name;
  String authority;
  String url;
  String licence;

  StudentCertificationsList(
      {Key key,
      this.id,
      this.studentId,
      this.name,
      this.authority,
      this.url,
      this.licence})
      : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentConferencesList extends StatelessWidget {
  String id;
  String studentId;
  String title;

  String organizer;
  String year;
  String address;
  String description;

  StudentConferencesList(
      {Key key,
      this.id,
      this.studentId,
      this.title,
      this.organizer,
      this.year,
      this.address,
      this.description})
      : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentTestsList extends StatelessWidget {
  String id;
  String studentId;
  String title;
  String year;
  String obtain;
  String total;

  StudentTestsList({
    Key key,
    this.id,
    this.studentId,
    this.title,
    this.year,
    this.obtain,
    this.total,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentPatentsList extends StatelessWidget {
  String id;
  String studentId;
  String title;
  String office;
  String app_number;
  String status;
  String date;
  String description;

  StudentPatentsList({
    Key key,
    this.id,
    this.studentId,
    this.title,
    this.office,
    this.app_number,
    this.status,
    this.date,
    this.description,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentCompetitionsList extends StatelessWidget {
  String id;
  String studentId;
  String title;

  String position;
  String year;
  String college;

  StudentCompetitionsList({
    Key key,
    this.id,
    this.studentId,
    this.title,
    this.position,
    this.year,
    this.college,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentPublicationsList extends StatelessWidget {
  String id;
  String studentId;
  String title;
  String publisher;
  String url;
  String year;
  String description;

  StudentPublicationsList({
    Key key,
    this.id,
    this.studentId,
    this.title,
    this.publisher,
    this.url,
    this.year,
    this.description,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentScholarshipsList extends StatelessWidget {
  String id;
  String studentId;
  String title;
  String year;

  String associated;

  String description;

  StudentScholarshipsList({
    Key key,
    this.id,
    this.studentId,
    this.title,
    this.year,
    this.associated,
    this.description,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentCurriculumsList extends StatelessWidget {
  String id;
  String studentId;
  String title;
  String description;

  StudentCurriculumsList({
    Key key,
    this.id,
    this.studentId,
    this.title,
    this.description,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

class StudentInternshipList extends StatelessWidget {
  String id;
  String studentId;
  String title;
  String company;
  String location;
  String start;
  String end;
  String salary;
  String description;

  StudentInternshipList(
      {Key key,
      this.id,
      this.studentId,
      this.title,
      this.company,
      this.location,
      this.start,
      this.end,
      this.salary,
      this.description})
      : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

