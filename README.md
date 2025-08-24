# Digital-earning
We are creating for learning and earning 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Digital Earning</title>
  <style>
    body {font-family: Arial, sans-serif; margin:0; padding:0;
      background: linear-gradient(135deg,#4facfe,#00f2fe); color:#fff;}
    header {background: rgba(0,0,0,0.6); padding:1rem; text-align:center; position:sticky; top:0; z-index:100;}
    nav a {margin:0 10px; color:#fff; text-decoration:none; font-weight:bold; cursor:pointer;}
    section {padding:2rem; text-align:center; display:none;}
    .card {background: rgba(255,255,255,0.1); padding:1rem; border-radius:12px; margin:1rem auto; max-width:500px; box-shadow:0 4px 10px rgba(0,0,0,0.3);}
    .card img {max-width:100%; border-radius:8px;}
    .card video {width:100%; border-radius:8px; margin-top:0.5rem;}
    .btn {display:inline-block; padding:0.7rem 1.5rem; margin-top:1rem; background:#fff; color:#333; border-radius:8px; text-decoration:none; font-weight:bold; cursor:pointer;}
    footer {background: rgba(0,0,0,0.7); text-align:center; padding:1rem; margin-top:2rem;}
    #adminPanel {display:none; background: rgba(0,0,0,0.7); padding:2rem;}
    input, textarea {width:90%; padding:0.5rem; margin:0.5rem 0; border-radius:6px; border:none;}
    .link {color:yellow; cursor:pointer; font-size:0.9em;}
  </style>
</head>
<body>
  <header>
    <h1>🌐 Digital Earning</h1>
    <nav>
      <a onclick="showSection('home')">Home</a>
      <a onclick="showSection('courses')">Courses</a>
      <a onclick="showSection('pricing')">Pricing</a>
      <a onclick="showSection('community')">Community</a>
      <a onclick="showAdmin()">Admin</a>
    </nav>
  </header>

  <section id="home">
    <h2>Welcome to Digital Earning</h2>
    <p>Learn skills, grow your income, and build your digital future 🚀</p>
    <a class="btn" onclick="showSection('courses')">Get Started</a>
  </section>

  <section id="courses">
    <h2>📚 Our Courses</h2>
    <div id="courseList"></div>
  </section>

  <section id="pricing">
    <h2>💎 Pricing Plans</h2>
    <div id="pricingList"></div>
  </section>

  <section id="community">
    <h2>👥 Community</h2>
    <p>Join our learning group and grow together with thousands of learners.</p>
    <a class="btn" href="#">Join Now</a>
  </section>

  <section id="adminPanel">
    <h2>🔑 Admin Panel</h2>

    <div id="adminLogin">
      <input type="email" id="loginEmail" placeholder="Enter Email">
      <input type="password" id="loginPassword" placeholder="Enter Password">
      <button class="btn" onclick="login()">Login</button>
      <p class="link" onclick="showReset()">Forgot Password?</p>
    </div>

    <div id="resetPassword" style="display:none;">
      <h3>Reset Password</h3>
      <input type="email" id="resetEmail" placeholder="Enter Your Email">
      <input type="password" id="newPassword" placeholder="Enter New Password">
      <button class="btn" onclick="resetPassword()">Reset</button>
      <p class="link" onclick="backToLogin()">Back to Login</p>
    </div>

    <div id="adminContent" style="display:none;">
      <h3>Add Course</h3>
      <input type="text" id="courseTitle" placeholder="Course Title">
      <textarea id="courseDesc" placeholder="Course Description"></textarea>
      <input type="text" id="courseVideo" placeholder="Video URL (YouTube/MP4)">
      <input type="text" id="courseImage" placeholder="Image URL">
      <button class="btn" onclick="addCourse()">Add Course</button>

      <h3>Add Pricing</h3>
      <input type="text" id="planName" placeholder="Plan Name">
      <textarea id="planDesc" placeholder="Plan Description"></textarea>
      <input type="text" id="planPrice" placeholder="Plan Price">
      <button class="btn" onclick="addPlan()">Add Plan</button>

      <button class="btn" onclick="logout()">Logout</button>
    </div>
  </section>

  <footer>
    <p>© 2025 Digital Earning. All Rights Reserved.</p>
  </footer>

  <script>
    const defaultCourses = [
      {title:"Digital Marketing", desc:"Master SEO, Ads, and Social Media Marketing.", video:"", image:""},
      {title:"Affiliate Marketing", desc:"Learn how to earn money promoting products online.", video:"", image:""},
    ];
    const defaultPlans = [
      {name:"Silver", desc:"Basic access to starter courses", price:"NRs. 1,999"},
      {name:"Gold", desc:"Full access to all courses", price:"NRs. 4,999"},
    ];

    // Pre-set admin
    let users = JSON.parse(localStorage.getItem("users")) || [
      {email:"bipingautamgautam474@gmail.com", password:"password12345678"}
    ];
    let courses = JSON.parse(localStorage.getItem("courses")) || defaultCourses;
    let plans = JSON.parse(localStorage.getItem("plans")) || defaultPlans;
    let currentUser = null;

    function renderCourses(){
      const container = document.getElementById("courseList");
      container.innerHTML = "";
      courses.forEach((c,i)=>{
        container.innerHTML+=`
          <div class="card">
            <h3>${c.title}</h3>
            <p>${c.desc}</p>
            ${c.image?`<img src="${c.image}" alt="${c.title}">`:""}
            ${c.video?`<video controls src="${c.video}"></video>`:""}
            ${currentUser?`<button class="btn" onclick="deleteCourse(${i})">Delete</button>`:""}
          </div>`;
      });
    }

    function renderPlans(){
      const container = document.getElementById("pricingList");
      container.innerHTML = "";
      plans.forEach((p,i)=>{
        container.innerHTML+=`
          <div class="card">
            <h3>${p.name}</h3>
            <p>${p.desc}</p>
            <p><strong>${p.price}</strong></p>
            ${currentUser?`<button class="btn" onclick="deletePlan(${i})">Delete</button>`:""}
          </div>`;
      });
    }

    function addCourse(){
      const title = document.getElementById("courseTitle").value;
      const desc = document.getElementById("courseDesc").value;
      const video = document.getElementById("courseVideo").value;
      const image = document.getElementById("courseImage").value;
      if(title && desc){
        courses.push({title, desc, video, image});
        localStorage.setItem("courses", JSON.stringify(courses));
        renderCourses();
        alert("Course Added!");
      } else alert("Title & Description required");
    }

    function addPlan(){
      const name = document.getElementById("planName").value;
      const desc = document.getElementById("planDesc").value;
      const price = document.getElementById("planPrice").value;
      if(name && desc && price){
        plans.push({name, desc, price});
        localStorage.setItem("plans", JSON.stringify(plans));
        renderPlans();
        alert("Plan Added!");
      } else alert("All fields required");
    }

    function deleteCourse(i){ courses.splice(i,1); localStorage.setItem("courses",JSON.stringify(courses)); renderCourses(); }
    function deletePlan(i){ plans.splice(i,1); localStorage.setItem("plans",JSON.stringify(plans)); renderPlans(); }

    function login(){
      const email = document.getElementById("loginEmail").value;
      const password = document.getElementById("loginPassword").value;
      const user = users.find(u=>u.email===email && u.password===password);
      if(user){
        currentUser=user;
        document.getElementById("adminLogin").style.display="none";
        document.getElementById("resetPassword").style.display="none";
        document.getElementById("adminContent").style.display="block";
        renderCourses();
        renderPlans();
        alert("Login Successful!");
      } else alert("Invalid Email or Password");
    }

    function logout(){ currentUser=null; document.getElementById("adminLogin").style.display="block"; document.getElementById("adminContent").style.display="none"; }

    function showReset(){ document.getElementById("adminLogin").style.display="none"; document.getElementById("resetPassword").style.display="block"; }
    function backToLogin(){ document.getElementById("resetPassword").style.display="none"; document.getElementById("adminLogin").style.display="block"; }
    function resetPassword(){
      const email=document.getElementById("resetEmail").value;
      const newPass=document.getElementById("newPassword").value;
      let user=users.find(u=>u.email===email);
      if(user){ user.password=newPass; localStorage.setItem("users",JSON.stringify(users)); alert("Password Reset Successful!"); backToLogin(); } 
      else alert("No user found with this Email");
    }

    function showSection(id){ document.querySelectorAll("section").forEach(s=>s.style.display="none"); document.getElementById(id).style.display="block"; }
    function showAdmin(){ showSection("adminPanel"); }

    renderCourses(); renderPlans(); showSection("home");
  </script>
</body>
</html>
