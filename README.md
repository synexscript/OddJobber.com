
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>OddJobber — Preview</title>
<style>
  :root{
    --bg:#f8fafc; --card:#ffffff; --primary:#0ea5e9; --accent:#10b981; --muted:#64748b;
    --danger:#ef4444; --glass: rgba(255,255,255,0.6);
  }
  *{box-sizing:border-box;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,Arial;}
  body{margin:0;background:linear-gradient(180deg,var(--bg),#eef2ff);color:#0f172a}
  .container{max-width:1100px;margin:28px auto;padding:18px}
  header{display:flex;align-items:center;justify-content:space-between;gap:12px}
  h1{margin:0;font-size:20px}
  nav{display:flex;gap:8px}
  button{cursor:pointer;border:0;padding:8px 12px;border-radius:8px;background:var(--primary);color:#fff}
  .muted{color:var(--muted);font-size:13px}
  .card{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 6px 18px rgba(2,6,23,0.06);margin-top:14px}
  .grid{display:grid;gap:12px}
  .grid-cols-3{grid-template-columns:1fr 1fr 1fr}
  input,select,textarea{width:100%;padding:8px;border-radius:8px;border:1px solid #e6eef8;background:#fff}
  .job{padding:12px;border-radius:10px;border:1px solid #eef2ff;background:linear-gradient(180deg,#fff,#fbfdff)}
  .small{font-size:13px}
  .actions{display:flex;gap:8px;margin-top:8px}
  .ghost{background:transparent;border:1px solid #e6eef8;color:#0f172a}
  .green{background:var(--accent);color:#fff}
  .danger{background:var(--danger);color:#fff}
  .topline{display:flex;gap:8px;align-items:center}
  .logo{font-weight:700;color:var(--primary);font-size:18px}
  footer{margin-top:30px;text-align:center;color:var(--muted);font-size:13px}
  .modal{position:fixed;inset:0;background:rgba(2,6,23,0.4);display:flex;align-items:center;justify-content:center}
  .modal .card{max-width:480px;width:96%}
  .hidden{display:none}
  .chip{display:inline-block;padding:6px 8px;border-radius:999px;background:#f1f5f9;font-size:12px;border:1px solid #e2e8f0}
  .row{display:flex;gap:12px;align-items:center}
  .spacer{flex:1}
  .small-muted{font-size:12px;color:var(--muted)}
  .pill{padding:6px 8px;border-radius:999px;background:#eef2ff;color:var(--primary);font-weight:600;font-size:13px}
  .center{text-align:center}
</style>
</head>
<body>

<div class="container">
  <header>
    <div class="topline">
      <div class="logo">OddJobber</div>
      <div class="muted small" style="margin-left:8px">Earn from small jobs around you</div>
    </div>
    <nav id="nav"></nav>
  </header>

  <main id="app">
    <!-- Content injected by JS -->
  </main>

  <footer class="muted small">This is a local demo preview. No external services are used.</footer>
</div>

<!-- Modals -->
<div id="otpModal" class="modal hidden" aria-hidden="true">
  <div class="card">
    <h3>OTP Verification</h3>
    <p class="small-muted">We sent a 6-digit code to <span id="otpPhone"></span></p>
    <input id="otpInput" placeholder="Enter OTP (try 123456)" />
    <div style="margin-top:10px;display:flex;gap:8px;">
      <button id="verifyOtp">Verify</button>
      <button id="autoVerify" class="ghost">Auto-verify (demo)</button>
      <button id="closeOtp" class="ghost">Close</button>
    </div>
  </div>
</div>

<div id="upiModal" class="modal hidden" aria-hidden="true">
  <div class="card">
    <h3>Simulate UPI Payment</h3>
    <p class="small-muted">This is a mock UPI checkout to preview commission handling.</p>
    <div style="margin-top:8px">
      <div class="row small-muted"><div>Job:</div><div id="upiJobTitle" class="spacer"></div><div id="upiJobAmount"></div></div>
      <div class="row" style="margin-top:10px">
        <div><label>UPI ID</label><input id="upiId" placeholder="eg: mobilenumber@upi" /></div>
      </div>
      <div style="margin-top:10px" class="small-muted">Commission: <span id="upiCommission"></span></div>
      <div style="margin-top:12px;display:flex;gap:8px">
        <button id="confirmPay" class="green">Simulate Pay</button>
        <button id="cancelPay" class="ghost">Cancel</button>
      </div>
    </div>
  </div>
</div>

<script>
/*
  OddJobber - Single-file preview app
  - Uses localStorage to persist users, profiles, jobs, applications, payments
  - OTP is simulated (code 123456) or Auto-verify
  - UPI payment is simulated with commission (5%)
*/

// ---------- Utilities ----------
const $ = (sel, root=document) => root.querySelector(sel);
const $$ = (sel, root=document) => Array.from(root.querySelectorAll(sel));
const uid = () => (Date.now().toString(36) + Math.random().toString(36).slice(2,8));

// ---------- Seed data keys ----------
const STORAGE_KEYS = {
  USERS: 'ojb_users',        // auth-like: {phone, id}
  PROFILES: 'ojb_profiles',  // profiles keyed by id
  JOBS: 'ojb_jobs',
  APPS: 'ojb_apps',
  PAYMENTS: 'ojb_payments',
  SESSION: 'ojb_session'     // {userId}
};

// ---------- Default seed (3 users, 5 jobs) ----------
function seedIfEmpty(){
  if (!localStorage.getItem(STORAGE_KEYS.USERS)) {
    const users = [
      { id: 'b1111111-1111', phone: '+919876543210' },
      { id: 'c2222222-2222', phone: '+919812345678' },
      { id: 'd3333333-3333', phone: '+919700000000' },
    ];
    localStorage.setItem(STORAGE_KEYS.USERS, JSON.stringify(users));
  }
  if (!localStorage.getItem(STORAGE_KEYS.PROFILES)) {
    const profiles = {
      'b1111111-1111': { id:'b1111111-1111', full_name: 'Ramesh Kumar', phone: '+919876543210', role:'poster', wallet_balance:100 },
      'c2222222-2222': { id:'c2222222-2222', full_name: 'Manish Singh', phone: '+919812345678', role:'seeker', wallet_balance:50 },
      'd3333333-3333': { id:'d3333333-3333', full_name: 'Sita Devi', phone: '+919700000000', role:'seeker', wallet_balance:10 },
    };
    localStorage.setItem(STORAGE_KEYS.PROFILES, JSON.stringify(profiles));
  }
  if (!localStorage.getItem(STORAGE_KEYS.JOBS)) {
    const jobs = [
      { id: uid(), poster_id:'b1111111-1111', title:'Paint store wall', description:'Paint 220 sq ft shop wall, 1 day', category:'Painting', pay:1500, location_text:'Jaipur, Rajasthan', expires_at: new Date(Date.now()+3*86400000).toISOString(), created_at:new Date().toISOString() },
      { id: uid(), poster_id:'b1111111-1111', title:'Harvest help (1 day)', description:'Help harvest wheat field for half day', category:'Farm', pay:800, location_text:'Alwar, Rajasthan', expires_at: new Date(Date.now()+5*86400000).toISOString(), created_at:new Date().toISOString() },
      { id: uid(), poster_id:'b1111111-1111', title:'Math tuition', description:'Teach class 6 maths for 2hrs', category:'Tutoring', pay:300, location_text:'Local', expires_at: new Date(Date.now()+7*86400000).toISOString(), created_at:new Date().toISOString() },
      { id: uid(), poster_id:'b1111111-1111', title:'Bike delivery', description:'Deliver small parcels', category:'Delivery', pay:200, location_text:'Jaipur', expires_at: new Date(Date.now()+2*86400000).toISOString(), created_at:new Date().toISOString() },
      { id: uid(), poster_id:'b1111111-1111', title:'Fix roof leak', description:'Repair small rooftop leak', category:'Repairs', pay:1200, location_text:'Jaipur', expires_at: new Date(Date.now()+6*86400000).toISOString(), created_at:new Date().toISOString() },
    ];
    localStorage.setItem(STORAGE_KEYS.JOBS, JSON.stringify(jobs));
  }
  if (!localStorage.getItem(STORAGE_KEYS.APPS)) localStorage.setItem(STORAGE_KEYS.APPS, JSON.stringify([]));
  if (!localStorage.getItem(STORAGE_KEYS.PAYMENTS)) localStorage.setItem(STORAGE_KEYS.PAYMENTS, JSON.stringify([]));
}

// ---------- Storage helpers ----------
const storage = {
  get(key){ const v = localStorage.getItem(key); return v? JSON.parse(v): null; },
  set(key, value){ localStorage.setItem(key, JSON.stringify(value)); }
};

// ---------- Auth / Session ----------
function currentSession(){ return storage.get(STORAGE_KEYS.SESSION); }
function currentUser(){
  const s = currentSession();
  if(!s) return null;
  const users = storage.get(STORAGE_KEYS.USERS) || [];
  return users.find(u=>u.id===s.userId) || null;
}

function createOrGetUserByPhone(phone){
  const users = storage.get(STORAGE_KEYS.USERS) || [];
  const found = users.find(u=>u.phone === phone);
  if(found) return found;
  const newUser = { id: uid(), phone };
  users.push(newUser);
  storage.set(STORAGE_KEYS.USERS, users);
  // create profile row
  const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
  profiles[newUser.id] = { id:newUser.id, full_name: phone, phone, role:'seeker', wallet_balance:0 };
  storage.set(STORAGE_KEYS.PROFILES, profiles);
  return newUser;
}

// session sign in
function signInByPhone(phone){
  const user = createOrGetUserByPhone(phone);
  storage.set(STORAGE_KEYS.SESSION, { userId: user.id });
  // ensure profile exists
  const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
  if(!profiles[user.id]) profiles[user.id] = { id:user.id, full_name: phone, phone, role: 'seeker', wallet_balance:0 };
  storage.set(STORAGE_KEYS.PROFILES, profiles);
  renderApp();
}

// sign out
function signOut(){
  localStorage.removeItem(STORAGE_KEYS.SESSION);
  renderApp();
}

// ---------- App UI / Routing ----------
const navEl = $('#nav');
const appEl = $('#app');
const otpModal = $('#otpModal');
const otpPhoneSpan = $('#otpPhone');
const otpInput = $('#otpInput');

let pendingOtpPhone = null;
let pendingSelectedRole = 'seeker';

// Setup nav buttons
function renderNav(){
  const user = currentUser();
  navEl.innerHTML = '';
  if(user){
    const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
    const p = profiles[user.id];
    const label = p?.full_name ? p.full_name : (user.phone || 'Me');
    navEl.innerHTML = `
      <div class="row">
        <div class="chip">${label}</div>
        <button class="ghost" id="navJobs">Find Jobs</button>
        <button class="ghost" id="navPost">Post Job</button>
        <button class="ghost" id="navWallet">Wallet</button>
        <button class="danger" id="navSignOut">Sign Out</button>
      </div>
    `;
    $('#navSignOut').onclick = signOut;
    $('#navJobs').onclick = ()=>route('jobs');
    $('#navPost').onclick = ()=>route('post');
    $('#navWallet').onclick = ()=>route('wallet');
  } else {
    navEl.innerHTML = `<button id="navLogin">Login / Signup</button>`;
    $('#navLogin').onclick = ()=>route('login');
  }
}

// Basic router: login, dashboard, jobs (list), post, wallet, profile
function route(page){
  window.location.hash = page;
  renderApp();
}

function renderApp(){
  renderNav();
  const page = (window.location.hash || '#').replace('#','') || 'login';
  if(!currentUser() && page !== 'login') {
    route('login');
    return;
  }
  if(page === 'login') renderLogin();
  else if(page === 'jobs') renderJobs();
  else if(page === 'post') renderPostJob();
  else if(page === 'wallet') renderWallet();
  else if(page === 'dashboard') renderDashboard();
  else if(page === 'profile') renderProfile();
  else renderLogin();
}

// ---------- Pages ----------
function renderLogin(){
  appEl.innerHTML = `
    <div class="card" style="max-width:720px;margin:auto">
      <div style="display:flex;gap:12px;align-items:center">
        <div style="flex:1">
          <h2>Login / Signup (OTP)</h2>
          <p class="muted small">Enter your phone number. OTP is simulated for preview. Use 123456 or Auto-verify.</p>
          <div style="margin-top:10px" class="grid grid-cols-3">
            <div><input id="phoneInput" placeholder="9876543210" /></div>
            <div>
              <select id="roleSelect">
                <option value="seeker">Job Seeker</option>
                <option value="poster">Job Poster</option>
              </select>
            </div>
            <div><button id="sendOtp">Send OTP</button></div>
          </div>
        </div>
        <div style="width:260px">
          <div class="card center">
            <strong>Demo accounts</strong>
            <p class="small-muted" style="margin-top:6px">Use seeded demo numbers:</p>
            <div style="display:flex;flex-direction:column;gap:6px;margin-top:8px">
              <button class="ghost" data-phone="+919876543210">Ramesh (poster)</button>
              <button class="ghost" data-phone="+919812345678">Manish (seeker)</button>
              <button class="ghost" data-phone="+919700000000">Sita (seeker)</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  `;
  // handlers
  $('#sendOtp').onclick = ()=>{
    const phoneRaw = $('#phoneInput').value.trim();
    if(!phoneRaw){ alert('Enter phone'); return; }
    const phone = phoneRaw.startsWith('+')?phoneRaw:'+91'+phoneRaw.replace(/^0+/, '');
    pendingOtpPhone = phone;
    pendingSelectedRole = $('#roleSelect').value;
    otpPhoneSpan.textContent = phone;
    otpInput.value = '';
    otpModal.classList.remove('hidden');
    otpModal.setAttribute('aria-hidden','false');
  };
  // demo preset handlers
  $$('.ghost[data-phone]').forEach(btn=> btn.onclick = (e)=> {
    const phone = e.target.getAttribute('data-phone');
    $('#phoneInput').value = phone.replace('+91','');
  });
}

function renderDashboard(){
  appEl.innerHTML = `
    <div>
      <div class="card">
        <h2>Dashboard</h2>
        <p class="small-muted">Quick links and stats.</p>
        <div style="display:flex;gap:12px;margin-top:12px">
          <div class="card" style="flex:1"><strong id="statJobs">0</strong><div class="small-muted">Jobs available</div></div>
          <div class="card" style="flex:1"><strong id="statApps">0</strong><div class="small-muted">Applications</div></div>
          <div class="card" style="flex:1"><strong id="statBalance">₹0</strong><div class="small-muted">Your balance</div></div>
        </div>
      </div>

      <div class="grid" style="grid-template-columns:1fr 340px;gap:12px;margin-top:12px">
        <div>
          <div class="card">
            <h3>Nearby Jobs</h3>
            <div id="dashJobs"></div>
          </div>
        </div>
        <div>
          <div class="card">
            <h4>Your Profile</h4>
            <div id="dashProfile"></div>
            <div style="margin-top:8px"><button class="ghost" onclick="route('profile')">View full profile</button></div>
          </div>
          <div class="card" style="margin-top:12px">
            <h4>Quick Actions</h4>
            <div style="display:flex;flex-direction:column;gap:8px;margin-top:8px">
              <button class="ghost" onclick="route('jobs')">Find Jobs</button>
              <button class="ghost" onclick="route('post')">Post Job</button>
              <button class="ghost" onclick="route('wallet')">Wallet</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  `;
  const jobs = storage.get(STORAGE_KEYS.JOBS)||[];
  $('#statJobs').textContent = jobs.length;
  const apps = storage.get(STORAGE_KEYS.APPS)||[];
  $('#statApps').textContent = apps.length;
  const user = currentUser();
  const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
  const p = profiles[user.id];
  $('#statBalance').textContent = `₹${(p?.wallet_balance||0)}`;
  // profile snippet
  $('#dashProfile').innerHTML = `
    <div><strong>${p?.full_name||p?.phone||'You'}</strong></div>
    <div class="small-muted">Role: ${p?.role}</div>
    <div class="small-muted">Phone: ${p?.phone}</div>
  `;
  // jobs
  const container = $('#dashJobs');
  container.innerHTML = '';
  jobs.slice(0,6).forEach(job=>{
    const el = document.createElement('div'); el.className='job'; 
    el.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:start">
      <div><strong>${job.title}</strong><div class="small-muted">${job.category} • ${job.location_text}</div><div style="margin-top:6px">${job.description}</div></div>
      <div style="text-align:right"><div class="pill">₹${job.pay}</div><div class="small-muted" style="margin-top:6px">Expires ${new Date(job.expires_at).toLocaleDateString()}</div></div>
    </div>
    <div class="actions"><button class="ghost" onclick="viewJob('${job.id}')">View</button><button class="green" onclick="openApply('${job.id}')">Apply</button></div>`;
    container.appendChild(el);
  });
}

function renderJobs(){
  const jobs = storage.get(STORAGE_KEYS.JOBS)||[];
  appEl.innerHTML = `
    <div>
      <div class="card">
        <h2>Find Jobs</h2>
        <div class="small-muted">Browse recent job listings near you (demo).</div>
      </div>
      <div class="grid" style="grid-template-columns:1fr 320px;gap:12px;margin-top:12px">
        <div>
          <div id="jobsList" class="card"></div>
        </div>
        <div>
          <div class="card">
            <h4>Filters</h4>
            <div style="margin-top:8px">
              <input id="filterText" placeholder="Search title, category, location" />
              <div style="margin-top:8px"><button id="applyFilter" class="ghost">Apply</button></div>
            </div>
          </div>
          <div class="card" style="margin-top:12px">
            <h4>Seeded Jobs</h4>
            <div class="small-muted">Demo data loaded</div>
          </div>
        </div>
      </div>
    </div>
  `;
  function listJobs(filter=''){
    const container = $('#jobsList');
    container.innerHTML = '';
    let out = jobs.slice().sort((a,b)=> new Date(b.created_at)-new Date(a.created_at));
    if(filter) out = out.filter(j=> (j.title+ ' ' + j.description + ' ' + j.category + ' ' + j.location_text).toLowerCase().includes(filter.toLowerCase()));
    out.forEach(job=>{
      const el = document.createElement('div'); el.className='job'; 
      el.innerHTML = `<div style="display:flex;justify-content:space-between">
        <div>
          <strong>${job.title}</strong><div class="small-muted">${job.category} • ${job.location_text}</div>
          <div style="margin-top:8px">${job.description}</div>
        </div>
        <div style="text-align:right"><div class="pill">₹${job.pay}</div><div class="small-muted" style="margin-top:6px">Expires ${new Date(job.expires_at).toLocaleDateString()}</div></div>
      </div>
      <div class="actions"><button class="ghost" onclick="viewJob('${job.id}')">View</button><button class="green" onclick="openApply('${job.id}')">Apply</button></div>`;
      container.appendChild(el);
    });
  }
  listJobs();
  $('#applyFilter').onclick = ()=> listJobs($('#filterText').value.trim());
}

function renderPostJob(){
  appEl.innerHTML = `
    <div class="card" style="max-width:720px;margin:auto">
      <h2>Post a Job</h2>
      <p class="small-muted">Create a job listing. You must be a Poster.</p>
      <div style="margin-top:10px">
        <input id="jobTitle" placeholder="Title" />
        <textarea id="jobDesc" placeholder="Description" style="margin-top:8px"></textarea>
        <div style="display:flex;gap:8px;margin-top:8px">
          <input id="jobCategory" placeholder="Category" />
          <input id="jobPay" placeholder="Pay (₹)" type="number" />
        </div>
        <input id="jobLocation" placeholder="Location (village / city)" style="margin-top:8px" />
        <div style="margin-top:12px"><button id="postJobBtn">Post Job</button></div>
      </div>
    </div>
  `;
  $('#postJobBtn').onclick = ()=>{
    const title = $('#jobTitle').value.trim();
    const desc = $('#jobDesc').value.trim();
    const category = $('#jobCategory').value.trim() || 'General';
    const pay = Number($('#jobPay').value) || 0;
    const location_text = $('#jobLocation').value.trim() || 'Local';
    if(!title || !desc){ alert('Please add title and description'); return; }
    const jobs = storage.get(STORAGE_KEYS.JOBS) || [];
    const user = currentUser();
    const newJob = { id: uid(), poster_id: user.id, title, description: desc, category, pay, location_text, created_at: new Date().toISOString(), expires_at: new Date(Date.now()+7*86400000).toISOString() };
    jobs.unshift(newJob);
    storage.set(STORAGE_KEYS.JOBS, jobs);
    alert('Job posted!');
    route('jobs');
  };
}

function renderWallet(){
  const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
  const user = currentUser();
  const p = profiles[user.id];
  appEl.innerHTML = `
    <div class="card" style="max-width:720px;margin:auto">
      <h2>Wallet</h2>
      <div style="display:flex;gap:12px;margin-top:12px">
        <div style="flex:1">
          <div class="card">
            <div class="row"><div><strong>Balance</strong></div><div class="spacer"></div><div>₹${p.wallet_balance || 0}</div></div>
            <div style="margin-top:8px" class="small-muted">You can simulate sending payment for a job. Commission 5% will be taken by OddJobber (demo).</div>
          </div>
          <div style="margin-top:12px" class="card">
            <h4>Pay for a Job (demo)</h4>
            <div style="display:flex;gap:8px">
              <select id="payJobSelect"></select>
              <button id="payForJob" class="green">Pay</button>
            </div>
          </div>
        </div>
        <div style="width:320px">
          <div class="card">
            <h4>Payments</h4>
            <div id="paymentsList" class="small-muted"></div>
          </div>
        </div>
      </div>
    </div>
  `;
  const jobs = storage.get(STORAGE_KEYS.JOBS) || [];
  const paySel = $('#payJobSelect');
  paySel.innerHTML = jobs.map(j=>`<option value="${j.id}">${j.title} — ₹${j.pay} • ${j.location_text}</option>`).join('');
  $('#payForJob').onclick = ()=> {
    const jobId = paySel.value;
    if(!jobId){ alert('Select job'); return; }
    openUPIModal(jobId);
  };
  refreshPayments();
}

function renderProfile(){
  const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
  const user = currentUser();
  const p = profiles[user.id];
  appEl.innerHTML = `
    <div class="card" style="max-width:720px;margin:auto">
      <h2>Profile</h2>
      <div style="display:flex;gap:12px">
        <div style="flex:1">
          <input id="profileName" value="${p.full_name || ''}" />
          <div style="display:flex;gap:8px;margin-top:8px">
            <input id="profilePhone" value="${p.phone || ''}" />
            <select id="profileRole">
              <option ${p.role==='seeker'?'selected':''} value="seeker">Seeker</option>
              <option ${p.role==='poster'?'selected':''} value="poster">Poster</option>
            </select>
          </div>
          <div style="margin-top:10px">
            <button id="saveProfile" class="green">Save</button>
          </div>
        </div>
        <div style="width:260px">
          <div class="card small-muted">User ID: ${p.id}</div>
        </div>
      </div>
    </div>
  `;
  $('#saveProfile').onclick = ()=>{
    const name = $('#profileName').value.trim();
    const phone = $('#profilePhone').value.trim();
    const role = $('#profileRole').value;
    const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
    profiles[user.id] = {...profiles[user.id], full_name:name || profiles[user.id].full_name, phone:phone || profiles[user.id].phone, role};
    storage.set(STORAGE_KEYS.PROFILES, profiles);
    alert('Profile saved');
    renderNav();
  };
}

// ---------- Job view / apply ----------
function viewJob(jobId){
  const jobs = storage.get(STORAGE_KEYS.JOBS) || [];
  const job = jobs.find(j=>j.id===jobId);
  if(!job) return alert('Job not found');
  const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
  const poster = profiles[job.poster_id];
  appEl.innerHTML = `
    <div class="card" style="max-width:820px;margin:auto">
      <div style="display:flex;justify-content:space-between">
        <div><h2>${job.title}</h2><div class="small-muted">${job.category} • ${job.location_text}</div></div>
        <div style="text-align:right"><div class="pill">₹${job.pay}</div><div class="small-muted">Expires ${new Date(job.expires_at).toLocaleDateString()}</div></div>
      </div>
      <div style="margin-top:12px">${job.description}</div>
      <div style="margin-top:12px" class="small-muted">Posted by: ${poster?.full_name || poster?.phone || 'Unknown'}</div>
      <div style="margin-top:12px" class="actions">
        <button class="green" onclick="openApply('${job.id}')">Apply</button>
        <button class="ghost" onclick="route('jobs')">Back to list</button>
      </div>
    </div>
  `;
}

function openApply(jobId){
  const message = prompt('Write a short message to the poster (demo):', 'I can do this job today.');
  if(message === null) return;
  const user = currentUser();
  const apps = storage.get(STORAGE_KEYS.APPS) || [];
  apps.push({ id: uid(), job_id: jobId, seeker_id: user.id, message, status: 'pending', created_at: new Date().toISOString()});
  storage.set(STORAGE_KEYS.APPS, apps);
  alert('Application sent (demo). Poster will be notified in a real app.');
  route('jobs');
}

// ---------- UPI mock flow ----------
function openUPIModal(jobId){
  const jobs = storage.get(STORAGE_KEYS.JOBS) || [];
  const job = jobs.find(j=>j.id===jobId);
  if(!job) return alert('Job not found');
  $('#upiModal').classList.remove('hidden'); $('#upiModal').setAttribute('aria-hidden','false');
  $('#upiJobTitle').textContent = job.title;
  $('#upiJobAmount').textContent = `₹${job.pay}`;
  const commission = Math.round(job.pay * 0.05);
  $('#upiCommission').textContent = `₹${commission} (5%)`;
  $('#upiModal').dataset.jobId = jobId;
  $('#upiId').value = '';
}

$('#cancelPay').onclick = ()=> { $('#upiModal').classList.add('hidden'); $('#upiModal').setAttribute('aria-hidden','true'); };

$('#confirmPay').onclick = ()=>{
  const jobId = $('#upiModal').dataset.jobId;
  const upi = $('#upiId').value.trim();
  const jobs = storage.get(STORAGE_KEYS.JOBS) || [];
  const job = jobs.find(j=>j.id===jobId);
  if(!job) return alert('Job missing');
  if(!upi) return alert('Enter UPI ID (demo)');
  // simulate payment success
  const commission = Math.round(job.pay * 0.05);
  const seekerAmount = job.pay - commission;
  // record payment
  const payments = storage.get(STORAGE_KEYS.PAYMENTS) || [];
  const payRecord = { id: uid(), job_id: job.id, poster_id: currentUser().id, seeker_id: job.poster_id, amount: job.pay, commission, upi, status:'SUCCESS', created_at: new Date().toISOString() };
  payments.unshift(payRecord);
  storage.set(STORAGE_KEYS.PAYMENTS, payments);
  // update balances: subtract from poster's wallet (if poster), add to seeker's wallet
  const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
  // in demo, poster may be current user (paying) or not - for simplicity, if current user is poster we'll subtract; else we won't
  const current = currentUser();
  if(profiles[current.id]) profiles[current.id].wallet_balance = Math.max(0,(profiles[current.id].wallet_balance||0) - job.pay);
  // The payment is to job.poster_id (the seeker in our simplified demo flows where poster posts job and seekers apply). 
  // For demo we credit the poster's wallet? Wait — correct flow: Poster posts job; seeker does job; poster pays seeker.
  // Here, job.poster_id is the poster; we should credit seeker, but we don't have seeker selection in pay flow (simple demo: credit the first seeker who applied or credit a demo seeker)
  // We'll credit a seeker if there's an application; else credit "Manish" seeded user as a demo seeker.
  const apps = storage.get(STORAGE_KEYS.APPS) || [];
  const appForJob = apps.find(a=>a.job_id===job.id);
  let creditedSeekerId = appForJob ? appForJob.seeker_id : 'c2222222-2222'; // default demo seeker
  // credit seeker
  if(!profiles[creditedSeekerId]) profiles[creditedSeekerId] = { id:creditedSeekerId, full_name:'Demo Seeker', phone:'', role:'seeker', wallet_balance:0 };
  profiles[creditedSeekerId].wallet_balance = (profiles[creditedSeekerId].wallet_balance || 0) + seekerAmount;
  // store commission record (simple)
  const commissionRecords = storage.get(STORAGE_KEYS.PAYMENTS) || [];
  storage.set(STORAGE_KEYS.PAYMENTS, payments);
  storage.set(STORAGE_KEYS.PROFILES, profiles);
  $('#upiModal').classList.add('hidden'); $('#upiModal').setAttribute('aria-hidden','true');
  alert(`Payment simulated!\nTotal: ₹${job.pay}\nCommission(5%): ₹${commission}\nCredited seeker: ₹${seekerAmount}`);
  renderWallet();
};

// refresh payments list in wallet
function refreshPayments(){
  const payments = storage.get(STORAGE_KEYS.PAYMENTS) || [];
  $('#paymentsList').innerHTML = payments.slice(0,6).map(p=>`<div style="padding:8px;border-bottom:1px solid #f1f5f9"><div><strong>₹${p.amount}</strong> • ${p.status}</div><div class="small-muted">Job ${p.job_id} • ${new Date(p.created_at).toLocaleString()}</div></div>`).join('') || '<div class="small-muted">No payments yet</div>';
}

// ---------- OTP modal handlers ----------
$('#verifyOtp').onclick = ()=> {
  const code = otpInput.value.trim();
  if(code === '123456') {
    // sign in
    signInByPhone(pendingOtpPhone);
    otpModal.classList.add('hidden'); otpModal.setAttribute('aria-hidden','true');
    alert('OTP verified (demo). Profile created automatically (if new).');
    // set role if requested
    const profiles = storage.get(STORAGE_KEYS.PROFILES) || {};
    const u = storage.get(STORAGE_KEYS.USERS).find(x=>x.phone===pendingOtpPhone);
    if(u) {
      profiles[u.id] = profiles[u.id] || { id:u.id, full_name: pendingOtpPhone, phone: pendingOtpPhone, role: pendingSelectedRole, wallet_balance:0 };
      profiles[u.id].role = pendingSelectedRole;
      storage.set(STORAGE_KEYS.PROFILES, profiles);
    }
    renderApp();
  } else {
    alert('Wrong code in demo. Try 123456 or Auto-verify.');
  }
};

$('#autoVerify').onclick = ()=> {
  otpInput.value = '123456';
  $('#verifyOtp').click();
};

$('#closeOtp').onclick = ()=> { otpModal.classList.add('hidden'); otpModal.setAttribute('aria-hidden','true'); };

// ---------- init ----------
seedIfEmpty();

// initial route
if(!window.location.hash) window.location.hash = 'login';
renderApp();

// handle simple hash nav changes
window.addEventListener('hashchange', renderApp);

// Expose some functions for buttons
window.viewJob = viewJob;
window.openApply = openApply;
window.route = route;
</script>
</body>
</html>


---

If you want next:

I can convert this preview into a downloadable ZIP (HTML file already produced) or

Add a tiny static map preview, or

Add a mocked Razorpay popup that looks closer to the real one.


Which one should I do next?

