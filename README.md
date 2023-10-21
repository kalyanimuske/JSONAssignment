# JSONAssignment
'Title of the Project' : STUDENT ENROLLMENT FORM
'Description' : This is a student enrollment form which is created using java script object notation and hyper text markup language. The data filled in this is automatically tranfered into the database connected .
'Benefits of using' : Creating a student enrollment form using JavaScript and HTML can offer several benefits, depending on your specific use case and requirements such as it allows you to create dynamic and interactive user interfaces, Real-Time Validation, Improved User Guidance,Asynchronous Data Handling,Cost-Efficiency and much more.
'JsonPowerDB Release History' : JSON (JavaScript Object Notation) is a lightweight data-interchange format that has a simple and human-readable syntax. JSON was first introduced by Douglas Crockford in the early 2000s. It gained popularity for its simplicity and flexibility in representing structured data. JSON is often used in web development for data exchange between a server and a web application, and it has become a standard format for APIs.


<!DOCTYPE html>

<html>

<head>
    <title> JSON ASSIGNMENT </title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
    <script type="text/javascript" src="http://login2explore.com/jpdb/resources/js/0.0.3/jpdb-commons.js"></script>

</head>

<body>
    <div class="container">

        <div class="page-header text-center">
            <h1>Student Enrollment Form</h1>
        </div>

        <form id="StudentForm" method="get">
            <div class="form-group">
                <label>Roll Number</label>
                <input type="text" id="RollNumber" name="RollNumber" class="form-control"
                    placeholder="Enter Student Roll Number" onchange="getStudent()">
            </div>
            <div class="form-group">
                <label>Student Full Name</label>
                <input type="text" id="StudentFullName" name="StudentFullName" class="form-control"
                    placeholder="Enter Student Full Name">
            </div>
            <div class="form-group">
                <label>Class</label>
                <input type="text" id="StudentClass" name="StudentClass" class="form-control"
                    placeholder="Enter Student Class">
            </div>
            <div class="form-group">
                <label>Birth-Date</label>
                <input type="text" id="BirthDate" name="BirthDate" class="form-control"
                    placeholder="Enter Student Birth-Date as dd/mm/yyyy">
            </div>
            <div class="form-group">
                <label>Address</Address></label>
                <input type="text" id="Address" name="Address" class="form-control" placeholder="Enter Student Address">
            </div>
            <div class="form-group">
                <label>Enrollment-Date </label>
                <input type="text" id="EnrollmentDate" name="EnrollmentDate" class="form-control"
                    placeholder="Enter Student Enrollment-Date as dd/mm/yyyy">
            </div>

            <div class="form-group text-center">
                <button type="button" class="btn btn-lg btn-primary" id="save" onclick="saveData()" disabled> Save
                </button>
                <button type="button" class="btn btn-lg btn-primary" id="change" onclick="updateData()" disabled> Update
                </button>
                <button type="button" class="btn btn-lg btn-primary" id="reset" onclick="resetForm()" disabled> Reset
                </button>
            </div>

        </form>

    </div>


    <script type="text/javascript">


        $("RollNumber").focus();

        function recNo(jsonObj) {
            var n = JSON.parse(jsonObj.data);
            localStorage.setItem("recno", n.rec_no);
        }

        function checkID() {
            var RollNumber = $("#RollNumber").val();
            var jsonStr = {
                RollNumber: RollNumber
            };
            return JSON.stringify(jsonStr);
        }

        function fillData(jsonObj) {
            recNo(jsonObj);
            var data = JSON.parse(jsonObj.data).record;
            $("#StudentFullName").val(data.StudentFullName);
            $("#StudentClass").val(data.StudentClass);
            $("#BirthDate").val(data.BirthDate);
            $("#Address").val(data.Address);
            $("#EnrollmentDate").val(data.EnrollmentDate);
        }

        function resetForm() {
            $("#RollNumber").val("");
            $("#StudentFullName").val("");
            $("#StudentClass").val("");
            $("#BirthDate").val("");
            $("#Address").val("");
            $("#EnrollmentDate").val("");
            $("#RollNumber").prop("disabled", false);
            $("#save").prop("disabled", true);
            $("#change").prop("disabled", true);
            $("#reset").prop("disabled", true);
            $("#RollNumber").focus();
        }

        function validateData() {
            var RollNumber, StudentFullName, StudentClass, BirthDate, Address, EnrollmentDate;
            RollNumber = $("#RollNumber").val();
            StudentFullName = $("#StudentFullName").val();
            StudentClass = $("#StudentClass").val();
            BirthDate = $("#BirthDate").val();
            Address = $("#Address").val();
            EnrollmentDate = $("#EnrollmentDate").val();

            if (RollNumber === "") {
                alert("Student Roll Number Required Value");
                $("#RollNumber").focus();
                return "";
            }
            if (StudentFullName === "") {
                alert("Student Full Name is Required Valueg");
                $("#StudentFullName").focus();
                return "";
            }
            if (StudentClass === "") {
                alert("Student class is Required Value");
                $("#StudentClass").focus();
                return "";
            }
            if (BirthDate === "") {
                alert("Birth-Date is Required Value");
                $("#BirthDate").focus();
                return "";
            }
            if (Address === "") {
                alert("Address is Required Value");
                $("#Address").focus();
                return "";
            }
            if (EnrollmentDate === "") {
                alert("Enrollment-Date is Required Value");
                $("#EnrollmentDate").focus();
                return "";
            }

            var jsonStrObj = {
                RollNumber: RollNumber,
                StudentFullName: StudentFullName,
                StudentClass: StudentClass,
                BirthDate: BirthDate,
                Address: Address,
                EnrollmentDate: EnrollmentDate,
            };
            return JSON.stringify(jsonStrObj);
        }

        function getStudent() {
            var jsonObj = checkID();
            var getRequest = createGET_BY_KEYRequest("90931577|-31949333567860227|90960057", "SCHOOL-DB", "STUDENT-TABLE", jsonObj);
            jQuery.ajaxSetup({ async: false });
            var jsonObj = executeCommandAtGivenBaseUrl(getRequest, "http://api.login2explore.com:5577", "/api/irl");
            jQuery.ajaxSetup({ async: true });
            if (jsonObj.status === 400) {
                $("#save").prop("disabled", false);
                $("#reset").prop("disabled", false);
                $("#StudentFullName").focus();

            } else if (jsonObj.status === 200) {

                $("#RollNumber").prop("disabled", true);
                fillData(jsonObj);

                $("#change").prop("disabled", false);
                $("#reset").prop("disabled", false);
                $("#StudentFullName").focus();

            }
        }

        function saveData() {

            var jsonStrObj = validateData();
            if (jsonStrObj === "") {
                return "";
            }
            var putRequest = createPUTRequest("90931577|-31949333567860227|90960057", jsonStrObj, "SCHOOL-DB", "STUDENT-TABLE");
            jQuery.ajaxSetup({ async: false });
            var jsonObj = executeCommandAtGivenBaseUrl(putRequest, "http://api.login2explore.com:5577", "/api/iml");
            jQuery.ajaxSetup({ async: true });
            console.log(jsonObj);
            resetForm();
            $("#RollNumber").focus();

        }

        function updateData() {

            jsonChg = validateData();
            var updateRequest = createUPDATERecordRequest("90931577|-31949333567860227|90960057", jsonChg, "SCHOOL-DB", "STUDENT-TABLE", localStorage.getItem("recno"));
            jQuery.ajaxSetup({ async: false });
            var jsonObj = executeCommandAtGivenBaseUrl(updateRequest, "http://api.login2explore.com:5577", "/api/iml");
            jQuery.ajaxSetup({ async: true });
            console.log(jsonObj);
            resetForm();
            $("#RollNumber").focus();

        }
    </script>
</body>

</html>
