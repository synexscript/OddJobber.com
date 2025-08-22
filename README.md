<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>OddJobber Demo</title>
  <style>
    body { font-family: Arial, sans-serif; margin:0; padding:0; background:#f4f9f6; }
    header { background:#00796b; color:white; padding:1rem; text-align:center; }
    .container { padding:1rem; }
    .card { background:white; border-radius:12px; box-shadow:0 2px 6px rgba(0,0,0,0.1); padding:1rem; margin-bottom:1rem; }
    label { font-weight:bold; display:block; margin-top:0.5rem; }
    input, select, textarea, button { width:100%; padding:0.6rem; margin-top:0.3rem; border-radius:8px; border:1px solid #ccc; font-size:1rem; }
    button { background:#00796b; color:white; border:none; cursor:pointer; font-weight:bold; }
    button:disabled { background:#aaa; }
    .jobs { margin-top:1rem; }
    .job { border-bottom:1px solid #eee; padding:0.5rem 0; }
    .notice { background:#ffe0b2; padding:0.6rem; border-radius:8px; margin-bottom:1rem; }
    @media (max-width:600px) {
      body { font-size: 16px; }
      header { font-size: 18px; }
    }
  </style>
</head>
<body>
  <header>
    <h2>OddJobber Demo</h2>
    <p>Earn from small jobs around you</p>
  </header>
  <div class="container">
    <div id="subscription" class="card">
      <h3>Subscription Required</h3>
      <p>You need to pay <strong>₹150/month</strong> to access jobs.</p>
      <button onclick="activateSubscription()">Pay ₹150 Now</button>
    </div><div id="mainApp" style="display:none;">
  <div class="card">
    <h3>Post a Job</h3>
    <label>Job Title</label>
    <input type="text" id="jobTitle" />
    <label>Description</label>
    <textarea id="jobDesc"></textarea>
    <label>Category</label>
    <select id="jobCat">
      <option>Carpenter</option>
      <option>Mechanic</option>
      <option>Electrician</option>
      <option>Plumber</option>
      <option>Helper</option>
      <option>Agriculture</option>
      <option>Tutor</option>
      <option>Driver</option>
      <option>Painter</option>
      <option>Delivery</option>
      <option>Cook</option>
      <option>Gardener</option>
      <option>Housekeeping</option>
      <option>Welder</option>
      <option>Mason</option>
      <option>Tailor</option>
      <option>IT Support</option>
      <option>Data Entry</option>
      <option>Cleaning</option>
      <option>Watchman</option>
      <option>Packers & Movers</option>
      <option>AC Technician</option>
      <option>Beauty/Salon</option>
      <option>Baby Sitter</option>
      <option>Others</option>
    </select>
    <label>Payment (₹)</label>
    <input type="number" id="jobPay" />
    <button onclick="postJob()">Post Job</button>
  </div>

  <div class="card">
    <h3>Jobs Available</h3>
    <div id="jobList" class="jobs"></div>
  </div>

  <div class="card">
    <h3>Notifications</h3>
    <div id="notifications"></div>
  </div>
</div>

  </div>  <script>
    let subscribed = false;
    let jobs = [];
    function activateSubscription() {
      subscribed = true;
      document.getElementById('subscription').style.display='none';
      document.getElementById('mainApp').style.display='block';
      alert('✅ Subscription activated! You can now access OddJobber.');
    }
    function postJob(){
      if(!subscribed){ alert('Please pay ₹150 to continue'); return; }
      let title=document.getElementById('jobTitle').value;
      let desc=document.getElementById('jobDesc').value;
      let cat=document.getElementById('jobCat').value;
      let pay=document.getElementById('jobPay').value;
      if(title && desc && cat && pay){
        let job={title,desc,cat,pay};
        jobs.push(job);
        renderJobs();
        sendNotification(cat,title);
        document.getElementById('jobTitle').value='';
        document.getElementById('jobDesc').value='';
        document.getElementById('jobPay').value='';
      } else {
        alert('Fill all fields!');
      }
    }
    function renderJobs(){
      let jobList=document.getElementById('jobList');
      jobList.innerHTML='';
      jobs.forEach(j=>{
        jobList.innerHTML+=`<div class='job'><strong>${j.title}</strong> (${j.cat}) - ₹${j.pay}<br><small>${j.desc}</small></div>`;
      });
    }
    function sendNotification(cat,title){
      let n=document.getElementById('notifications');
      let note=document.createElement('div');
      note.className='notice';
      note.innerText=`🔔 New ${cat} job posted: ${title}`;
      n.prepend(note);
    }
  </script></body>
</html>