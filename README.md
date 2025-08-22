<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>OddJobber — Demo with Categories & Subscription (₹150)</title>
<style>
  :root{--bg:#f8fafc;--card:#fff;--primary:#0ea5e9;--accent:#10b981;--muted:#64748b;--danger:#ef4444}
  *{box-sizing:border-box;font-family:Inter,system-ui,Segoe UI,Roboto,Arial;}
  body{margin:0;background:linear-gradient(#f8fafc,#eef2ff);color:#06203a}
  .wrap{max-width:1100px;margin:28px auto;padding:18px}
  header{display:flex;align-items:center;justify-content:space-between}
  .logo{font-weight:700;color:var(--primary);font-size:20px}
  nav{display:flex;gap:8px}
  button{cursor:pointer;border:0;padding:8px 12px;border-radius:8px;background:var(--primary);color:#fff}
  .ghost{background:transparent;border:1px solid #e6eef8;color:#06203a}
  .danger{background:var(--danger)}
  .card{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 6px 18px rgba(2,6,23,0.06);margin-top:14px}
  .grid{display:grid;gap:12px}
  .grid-3{grid-template-columns:1fr 1fr 1fr}
  input,select,textarea{width:100%;padding:8px;border-radius:8px;border:1px solid #e6eef8;background:#fff}
  .job{padding:12px;border-radius:10px;border:1px solid #eef2ff;background:linear-gradient(#fff,#fbfdff);margin-bottom:10px}
  .small{font-size:13px;color:var(--muted)}
  .pill{display:inline-block;padding:6px 10px;border-radius:999px;background:#eef2ff;color:var(--primary);font-weight:600}
  .modal{position:fixed;inset:0;background:rgba(2,6,23,0.4);display:flex;align-items:center;justify-content:center}
  .hidden{display:none}
  .row{display:flex;gap:8px;align-items:center}
  .sp{flex:1}
  .notif-badge{background:red;color:#fff;padding:4px 7px;border-radius:999px;font-size:12px}
  label{font-size:13px;color:#334155}
  .muted{color:var(--muted)}
  footer{margin-top:20px;text-align:center;color:var(--muted);font-size:13px}
  ul{padding-left:18px;margin:0}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <div>
      <div class="logo">OddJobber — Demo</div>
      <div class="small muted">Categories, subscription (₹150), notifications demo</div>
    </div>
    <nav id="nav"></nav>
  </header>

  <main id="app"></main>

  <footer class="small muted">Local demo — no external services. Subscriptions & payments are simulated.</footer>
</div>

<!-- OTP Modal (demo) -->
<div id="otpModal" class="modal hidden" aria-hidden="true">
  <div class="card" style="max-width:520px;width:96%">
    <h3>OTP Verification (Demo)</h3>
    <p class="small muted">OTP simulated: use <strong>123456</strong> or Auto-verify.</p>
    <div class="row" style="margin-top:8px">
      <input id="otpInput" placeholder="Enter OTP (123456)" />
      <button id="verifyOtp">Verify</button>
      <button id="autoVerify" class="ghost">Auto-verify</button>
      <button id="closeOtp" class="ghost">Close</button>
    </div>
  </div>
</div>

<!-- Subscription Modal -->
<div id="subModal" class="modal hidden" aria-hidden="true">
  <div class="card" style="max-width:520px;width:96%">
    <h3>Subscribe to OddJobber — ₹150 / month</h3>
    <p class="small muted">Access full features: receive category notifications, apply to jobs, and be discoverable.</p>
    <div style="margin-top:8px">
      <div class="row" style="align-items:center">
        <div><label>Main plan</label><div class="pill">₹150 now → ₹150 monthly</div></div>
        <div class="sp"></div>
        <div><button id="paySub" class="green">Pay ₹150 (Simulate)</button></div>
      </div>
      <div style="margin-top:10px" class="small muted">Demo: clicking Pay will activate subscription and set next due date 30 days later.</div>
      <div style="margin-top:12px"><button id="closeSub" class="ghost">Close</button></div>
    </div>
  </div>
</div>

<!-- Notifications Modal -->
<div id="notifModal" class="modal hidden" aria-hidden="true">
  <div class="card" style="max-width:700px;width:96%">
    <h3>Your Notifications</h3>
    <div id="notifList" style="margin-top:8px"></div>
    <div style="margin-top:12px" class="row">
      <button id="clearNotif" class="ghost">Clear All</button>
      <div class="sp"></div>
      <button id="closeNotif" class="ghost">Close</button>
    </div>
  </div>
</div>

<script>
/* OddJobber demo with categories & subscription
   - Single-file app using localStorage
   - Users select worker categories in profile (multiple)
   - Posting a job asks for Main Category
   - Posting a job sends notifications to users who:
       - have that category in their profile AND
       - have an active subscription (paid ₹150)
   - Subscription simulated; pay ₹150 to activate (and demo monthly due date)
*/

// ---------------- storage keys ----------------
const KEYS = {
  USERS: 'ojb_users',
  PROFILES: 'ojb_profiles',
  JOBS: 'ojb_jobs',
  APPS: 'ojb_apps',
  PAYMENTS: 'ojb_payments',
  SESSION: 'ojb_session',
  NOTIFS: 'ojb_notifications'
};

// ---------------- categories list (25+) ----------------
const CATEGORIES = [
  'Carpenter','Mechanic','Helper','Agriculture','Electrician','Plumber','Painter','Mason','Tailor',
  'Tutor','Delivery','Driver','Cleaner','Cook','Gardener','Welder','Roofer','HVAC Technician','Pest Control',
  'Photographer','Event Staff','Babysitter','Animal Care','Laundry','Upholstery','CCTV Technician','Security Guard',
  'IT Support','Beauty & Salon','Plumber Assistant'
];

// ---------------- utils ----------------
const $ = (s, r=document) => r.querySelector(s);
const $$ = (s, r=document) => Array.from(r.querySelectorAll(s));
const uid = () => (Date.now().toString(36)+Math.random().toString(36).slice(2,8));
const fmtDate = ts => new Date(ts).toLocaleString();

// ---------------- storage helpers ----------------
const store = {
  get(k){ const v = localStorage.getItem(k); return v?JSON.parse(v):null; },
  set(k,v){ localStorage.setItem(k, JSON.stringify(v)); }
};

// ---------------- seed demo data ----------------
function seed(){
  if(!store.get(KEYS.USERS)){
    store.set(KEYS.USERS, [
      { id:'u_ramesh', phone:'+919876543210' },
      { id:'u_manish', phone:'+919812345678' },
      { id:'u_sita', phone:'+919700000000' }
    ]);
  }
  if(!store.get(KEYS.PROFILES)){
    const p = {
      'u_ramesh': { id:'u_ramesh', full_name:'Ramesh Kumar', phone:'+919876543210', role:'poster', categories:['Carpenter','Painter'], wallet_balance:100, subscription:{active:true,next_due: Date.now()+30*24*3600*1000} },
      'u_manish': { id:'u_manish', full_name:'Manish Singh', phone:'+919812345678', role:'seeker', categories:['Mechanic','Helper','Delivery'], wallet_balance:50, subscription:{active:true,next_due: Date.now()+15*24*3600*1000} },
      'u_sita': { id:'u_sita', full_name:'Sita Devi', phone:'+919700000000', role:'seeker', categories:['Agriculture','Helper'], wallet_balance:10, subscription:{active:false,next_due:null} }
    };
    store.set(KEYS.PROFILES, p);
  }
  if(!store.get(KEYS.JOBS)){
    store.set(KEYS.JOBS, [
      { id:uid(), poster_id:'u_ramesh', title:'Paint shop wall', main_category:'Painter', description:'Paint 200 sq ft shop, 1 day', pay:1500, location:'Jaipur', created_at:Date.now() },
      { id:uid(), poster_id:'u_ramesh', title:'Fix bike engine', main_category:'Mechanic', description:'Repair carburetor', pay:900, location:'Alwar', created_at:Date.now() }
    ]);
  }
  if(!store.get(KEYS.APPS)) store.set(KEYS.APPS, []);
  if(!store.get(KEYS.PAYMENTS)) store.set(KEYS.PAYMENTS, []);
  if(!store.get(KEYS.NOTIFS)) store.set(KEYS.NOTIFS, {});
}

// ---------------- session & auth ----------------
function currentSession(){ return store.get(KEYS.SESSION); }
function currentUser(){ const s = currentSession(); if(!s) return null; const users = store.get(KEYS.USERS)||[]; return users.find(u=>u.id===s.userId) || null; }

function createOrGetUserByPhone(phone){
  let users = store.get(KEYS.USERS)||[];
  let u = users.find(x=>x.phone===phone);
  if(u) return u;
  u = { id: 'u_'+uid(), phone };
  users.push(u);
  store.set(KEYS.USERS, users);
  // create default profile
  const profiles = store.get(KEYS.PROFILES)||{};
  profiles[u.id] = { id:u.id, full_name: phone, phone, role:'seeker', categories:[], wallet_balance:0, subscription:{active:false,next_due:null} };
  store.set(KEYS.PROFILES, profiles);
  return u;
}

function signInByPhone(phone){
  const user = createOrGetUserByPhone(phone);
  store.set(KEYS.SESSION, { userId: user.id });
  renderApp();
}

function signOut(){
  localStorage.removeItem(KEYS.SESSION);
  renderApp();
}

// ---------------- notifications ----------------
function notifyUsersForCategory(category, job){
  const profiles = store.get(KEYS.PROFILES) || {};
  const notifs = store.get(KEYS.NOTIFS) || {};
  Object.values(profiles).forEach(profile => {
    if(!profile.categories || profile.categories.length===0) return;
    if(profile.categories.includes(category) && profile.subscription && profile.subscription.active){
      const arr = notifs[profile.id] || [];
      arr.unshift({ id:uid(), text:`New ${category} job: ${job.title} • ₹${job.pay}`, job_id: job.id, ts: Date.now(), read:false });
      notifs[profile.id] = arr;
    }
  });
  store.set(KEYS.NOTIFS, notifs);
}

// ---------------- UI Rendering ----------------
const navEl = $('#nav');
const appEl = $('#app');
const otpModal = $('#otpModal');
const subModal = $('#subModal');
const notifModal = $('#notifModal');

// render nav
function renderNav(){
  navEl.innerHTML = '';
  const user = currentUser();
  if(user){
    const profiles = store.get(KEYS.PROFILES)||{};
    const prof = profiles[user.id] || {};
    const notifs = (store.get(KEYS.NOTIFS)||{})[user.id] || [];
    const unread = notifs.filter(n=>!n.read).length;
    navEl.innerHTML = `
      <div class="row" style="align-items:center">
        <div class="small muted" style="margin-right:12px">Hi, <strong>${prof.full_name || prof.phone || 'User'}</strong></div>
        <button class="ghost" id="navJobs">Find Jobs</button>
        <button class="ghost" id="navPost">Post Job</button>
        <button class="ghost" id="navProfile">Profile</button>
        <button class="ghost" id="navNotif">Notifications ${unread?'<span class="notif-badge">'+unread+'</span>':''}</button>
        <button class="ghost" id="navSub">Subscription</button>
        <button class="danger" id="navSignOut">Sign Out</button>
      </div>
    `;
    $('#navJobs').onclick = ()=> route('jobs');
    $('#navPost').onclick = ()=> route('post');
    $('#navProfile').onclick = ()=> route('profile');
    $('#navNotif').onclick = ()=> openNotifications();
    $('#navSub').onclick = ()=> openSubscription();
    $('#navSignOut').onclick = ()=> signOut();
  } else {
    navEl.innerHTML = `<button id="navLogin">Login / Signup</button>`;
    $('#navLogin').onclick = ()=> route('login');
  }
}

// simple router
function route(page){
  window.location.hash = page;
  renderApp();
}

function renderApp(){
  renderNav();
  const page = (window.location.hash || '#').replace('#','') || 'login';
  if(!currentUser() && page!=='login') { route('login'); return; }
  if(page==='login') renderLogin();
  else if(page==='jobs') renderJobs();
  else if(page==='post') renderPostJob();
  else if(page==='profile') renderProfile();
  else if(page==='subscription') renderSubscription();
  else renderJobs();
}

// Login page
function renderLogin(){
  appEl.innerHTML = `
    <div class="card" style="max-width:900px;margin:auto">
      <h2>Login / Signup (OTP demo)</h2>
      <div class="row" style="margin-top:8px">
        <input id="phoneInput" placeholder="Enter 10-digit phone (e.g. 9876543210)" />
        <select id="roleSelect"><option value="seeker">Job Seeker</option><option value="poster">Job Poster</option></select>
        <button id="sendOtp">Send OTP</button>
      </div>
      <div style="margin-top:10px" class="small muted">Demo seeded users: Ramesh (poster), Manish (seeker), Sita (seeker). Use seeded numbers or any number to create a demo account.</div>
      <div style="margin-top:8px">
        <button class="ghost" data-phone="+919876543210">Use Ramesh (poster)</button>
        <button class="ghost" data-phone="+919812345678">Use Manish (seeker)</button>
        <button class="ghost" data-phone="+919700000000">Use Sita (seeker)</button>
      </div>
    </div>
  `;
  $('#sendOtp').onclick = ()=>{
    const phoneRaw = $('#phoneInput').value.trim();
    if(!phoneRaw){ alert('Enter phone'); return; }
    const phone = phoneRaw.startsWith('+')?phoneRaw:('+91'+phoneRaw.replace(/^0+/, ''));
    // store selected role in temp so we can set profile role after OTP verify
    window.pendingRole = $('#roleSelect').value;
    window.pendingPhone = phone;
    otpModal.classList.remove('hidden'); otpModal.setAttribute('aria-hidden','false');
  };
  $$('button[data-phone]').forEach(b => b.onclick = e => {
    const phone = e.target.getAttribute('data-phone');
    $('#phoneInput').value = phone.replace('+91','');
  });
}

// Jobs page (view)
function renderJobs(){
  const jobs = store.get(KEYS.JOBS) || [];
  appEl.innerHTML = `
    <div>
      <div class="card"><h2>Find Jobs</h2><div class="small muted">Browse and apply (application requires active subscription).</div></div>
      <div style="display:flex;gap:12px;margin-top:12px">
        <div style="flex:1">
          <div class="card" id="jobsList"></div>
        </div>
        <div style="width:320px">
          <div class="card"><h4>Filters</h4>
            <label>Main category</label>
            <select id="filterCategory"><option value="">All</option>${CATEGORIES.map(c=>`<option>${c}</option>`).join('')}</select>
            <div style="margin-top:8px"><button id="applyFilter" class="ghost">Apply</button></div>
          </div>
          <div class="card" style="margin-top:12px"><h4>Legend</h4><div class="small muted">Only subscribed users receive notifications when jobs in their categories are posted.</div></div>
        </div>
      </div>
    </div>
  `;
  function list(catFilter=''){
    const container = $('#jobsList');
    container.innerHTML = '';
    let list = jobs.slice().sort((a,b)=>b.created_at-a.created_at);
    if(catFilter) list = list.filter(j=>j.main_category===catFilter);
    if(list.length===0) container.innerHTML = '<div class="small muted">No jobs</div>';
    list.forEach(j=>{
      const el = document.createElement('div'); el.className='job';
      el.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:start">
        <div><strong>${j.title}</strong><div class="small muted">${j.main_category} • ${j.location}</div><div style="margin-top:6px">${j.description}</div></div>
        <div style="text-align:right"><div class="pill">₹${j.pay}</div><div class="small muted" style="margin-top:6px">${new Date(j.created_at).toLocaleString()}</div></div>
      </div>
      <div style="margin-top:8px" class="row">
        <button class="ghost" onclick="viewJob('${j.id}')">View</button>
        <button class="green" onclick="attemptApply('${j.id}')">Apply</button>
      </div>`;
      container.appendChild(el);
    });
  }
  list('');
  $('#applyFilter').onclick = ()=> list($('#filterCategory').value);
}

// Post job page (asks main category)
function renderPostJob(){
  const user = currentUser();
  const profiles = store.get(KEYS.PROFILES)||{};
  const prof = profiles[user.id];
  appEl.innerHTML = `
    <div class="card" style="max-width:900px;margin:auto">
      <h2>Post a Job</h2>
      <p class="small muted">You must provide the <strong>Main Category</strong> (this is used to notify matching workers).</p>
      <div style="display:grid;gap:8px;margin-top:8px">
        <label>Title</label><input id="jobTitle" />
        <label>Description</label><textarea id="jobDesc"></textarea>
        <div style="display:flex;gap:8px">
          <div style="flex:1"><label>Main Category</label><select id="jobCategory">${CATEGORIES.map(c=>`<option>${c}</option>`).join('')}</select></div>
          <div style="width:140px"><label>Pay (₹)</label><input id="jobPay" type="number" /></div>
        </div>
        <label>Location</label><input id="jobLocation" placeholder="Village / City" />
        <div style="margin-top:8px"><button id="postJobBtn">Post Job & Notify</button></div>
      </div>
    </div>
  `;
  $('#postJobBtn').onclick = ()=>{
    const title = $('#jobTitle').value.trim();
    const desc = $('#jobDesc').value.trim();
    const mainCat = $('#jobCategory').value;
    const pay = Number($('#jobPay').value) || 0;
    const loc = $('#jobLocation').value.trim() || 'Local';
    if(!title || !desc || !mainCat){ alert('Please fill title, description and main category'); return; }
    const jobs = store.get(KEYS.JOBS)||[];
    const newJob = { id:uid(), poster_id: user.id, title, description:desc, main_category:mainCat, pay, location:loc, created_at: Date.now() };
    jobs.unshift(newJob); store.set(KEYS.JOBS, jobs);
    // create notifications for subscribed users in that category
    notifyUsersForCategory(mainCat, newJob);
    alert(`Job posted and notifications sent to subscribed ${mainCat} workers (demo).`);
    route('jobs');
  };
}

// Profile page: set categories and check subscription
function renderProfile(){
  const user = currentUser();
  const profiles = store.get(KEYS.PROFILES)||{};
  const prof = profiles[user.id];
  appEl.innerHTML = `
    <div class="card" style="max-width:900px;margin:auto">
      <h2>Profile</h2>
      <div style="display:flex;gap:12px">
        <div style="flex:1">
          <label>Full name</label><input id="pf_name" value="${escapeHtml(prof.full_name||'')}" />
          <label style="margin-top:8px">Phone</label><input id="pf_phone" value="${escapeHtml(prof.phone||'')}" />
          <label style="margin-top:8px">Role</label>
          <select id="pf_role"><option value="seeker"${prof.role==='seeker'?' selected':''}>Seeker</option><option value="poster"${prof.role==='poster'?' selected':''}>Poster</option></select>
          <div style="margin-top:8px"><label>Worker categories (select multiple if you do many)</label>
            <div style="height:120px;overflow:auto;border:1px solid #eef2ff;padding:8px;border-radius:8px" id="catsBox"></div>
          </div>
          <div style="margin-top:8px" class="row">
            <button id="saveProfile" class="green">Save Profile</button>
            <div class="sp"></div>
            <button id="openSubNow" class="ghost">Subscription</button>
          </div>
        </div>
        <div style="width:300px">
          <div class="card"><strong>Balance</strong><div class="small muted">₹${prof.wallet_balance||0}</div></div>
          <div class="card" style="margin-top:10px"><strong>Subscription</strong><div class="small muted" id="subInfo"></div></div>
        </div>
      </div>
    </div>
  `;
  // populate categories checkboxes
  const box = $('#catsBox');
  box.innerHTML = CATEGORIES.map(c=>{
    const checked = prof.categories && prof.categories.includes(c);
    return `<div><label><input type="checkbox" data-cat="${escapeHtml(c)}" ${checked?'checked':''}/> ${escapeHtml(c)}</label></div>`;
  }).join('');
  $('#saveProfile').onclick = ()=>{
    const name = $('#pf_name').value.trim();
    const phone = $('#pf_phone').value.trim();
    const role = $('#pf_role').value;
    const selected = Array.from(box.querySelectorAll('input[type=checkbox]:checked')).map(i=>i.getAttribute('data-cat'));
    prof.full_name = name || prof.full_name;
    prof.phone = phone || prof.phone;
    prof.role = role;
    prof.categories = selected;
    const profiles = store.get(KEYS.PROFILES)||{};
    profiles[user.id] = prof;
    store.set(KEYS.PROFILES, profiles);
    alert('Profile saved');
    renderNav();
  };
  $('#openSubNow').onclick = openSubscription;
  // sub info
  const subInfo = $('#subInfo');
  if(prof.subscription && prof.subscription.active){
    subInfo.innerHTML = `Active • Next due: ${new Date(prof.subscription.next_due).toLocaleString()}`;
  } else {
    subInfo.innerHTML = `Not active • <button id="subNowBtn" class="ghost">Subscribe ₹150</button>`;
    $('#subNowBtn').onclick = openSubscription;
  }
}

// Subscription modal open
function openSubscription(){
  subModal.classList.remove('hidden'); subModal.setAttribute('aria-hidden','false');
}

// perform simulated payment for subscription
$('#paySub')?.addEventListener?.('click', ()=> {
  const user = currentUser(); if(!user) { alert('Sign in first'); return; }
  const profiles = store.get(KEYS.PROFILES)||{}; const prof = profiles[user.id];
  // Simulate charging 150
  const payments = store.get(KEYS.PAYMENTS)||[]; payments.unshift({ id:uid(), type:'subscription', user_id:user.id, amount:150, ts:Date.now() }); store.set(KEYS.PAYMENTS, payments);
  // activate subscription
  const nextDue = Date.now() + 30*24*3600*1000; // +30 days
  prof.subscription = { active:true, next_due: nextDue };
  profiles[user.id] = prof; store.set(KEYS.PROFILES, profiles);
  alert('Subscription activated (demo). You will receive category notifications now.');
  subModal.classList.add('hidden'); subModal.setAttribute('aria-hidden','true');
  renderApp();
});

// subscription modal close
$('#closeSub')?.addEventListener?.('click', ()=> { subModal.classList.add('hidden'); subModal.setAttribute('aria-hidden','true'); });

// Notifications modal open
function openNotifications(){
  const user = currentUser(); if(!user) return alert('Login first');
  notifModal.classList.remove('hidden'); notifModal.setAttribute('aria-hidden','false');
  renderNotifications();
}
$('#closeNotif')?.addEventListener?.('click', ()=> { notifModal.classList.add('hidden'); notifModal.setAttribute('aria-hidden','true'); });
$('#clearNotif')?.addEventListener?.('click', ()=> { store.set(KEYS.NOTIFS, { ...(store.get(KEYS.NOTIFS)||{}) , [currentUser().id]: [] }); renderNotifications(); });

// render notifications for current user
function renderNotifications(){
  const user = currentUser(); if(!user) return;
  const notifs = (store.get(KEYS.NOTIFS)||{})[user.id] || [];
  const listEl = $('#notifList');
  if(!listEl) return;
  if(notifs.length===0) listEl.innerHTML = '<div class="small muted">No notifications</div>';
  else listEl.innerHTML = notifs.map(n=>`<div style="padding:8px;border-bottom:1px solid #f1f5f9"><div><strong>${escapeHtml(n.text)}</strong></div><div class="small muted">${fmtDate(n.ts)}</div></div>`).join('');
  // mark as read (simple)
  const all = store.get(KEYS.NOTIFS)||{}; if(all[user.id]) all[user.id] = all[user.id].map(x=>({...x, read:true})); store.set(KEYS.NOTIFS, all); renderNav();
}

// view a job (simple)
function viewJob(jobId){
  const jobs = store.get(KEYS.JOBS)||[]; const j = jobs.find(x=>x.id===jobId); if(!j) return alert('Job not found');
  appEl.innerHTML = `<div class="card" style="max-width:900px;margin:auto">
    <div style="display:flex;justify-content:space-between"><div><h2>${escapeHtml(j.title)}</h2><div class="small muted">${escapeHtml(j.main_category)} • ${escapeHtml(j.location)}</div></div><div class="pill">₹${j.pay}</div></div>
    <div style="margin-top:10px">${escapeHtml(j.description)}</div>
    <div style="margin-top:12px" class="row"><button class="green" onclick="attemptApply('${j.id}')">Apply</button><button class="ghost" onclick="route('jobs')">Back</button></div>
  </div>`;
}

// attempt to apply: require subscription if current user is seeker
function attemptApply(jobId){
  const user = currentUser();
  if(!user) return alert('Login first');
  const profiles = store.get(KEYS.PROFILES)||{}; const prof = profiles[user.id];
  if(prof.role !== 'seeker') return alert('Only Job Seekers can apply. Change role in profile.');
  // check subscription
  if(!prof.subscription || !prof.subscription.active){
    if(confirm('You need an active subscription (₹150/month) to apply and receive notifications. Subscribe now?')){
      openSubscription(); return;
    } else return;
  }
  const msg = prompt('Write a short message to the poster (demo):','I can do this job');
  if(msg===null) return;
  const apps = store.get(KEYS.APPS)||[]; apps.unshift({ id:uid(), job_id:jobId, seeker_id:user.id, message:msg, ts:Date.now(), status:'applied' }); store.set(KEYS.APPS, apps);
  alert('Application sent (demo).');
  route('jobs');
}

// Post-job notification logic handled when posting job (see renderPostJob)

// Profile helper: escape HTML
function escapeHtml(s){ return String(s||'').replace(/[&<>"']/g, m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }

// ---------------- OTP modal handlers ----------------
$('#verifyOtp').onclick = ()=> {
  const code = $('#otpInput').value.trim();
  if(code === '123456'){
    const phone = window.pendingPhone || '+919999999999';
    // create or get user
    const user = createOrGetUserByPhone(phone);
    // create profile if not exists and set role from pendingRole
    const profiles = store.get(KEYS.PROFILES)||{};
    profiles[user.id] = profiles[user.id] || { id:user.id, full_name:phone, phone, role: window.pendingRole || 'seeker', categories:[], wallet_balance:0, subscription:{active:false,next_due:null} };
    profiles[user.id].role = window.pendingRole || profiles[user.id].role;
    store.set(KEYS.PROFILES, profiles);
    // set session
    store.set(KEYS.SESSION, { userId: user.id });
    otpModal.classList.add('hidden'); otpModal.setAttribute('aria-hidden','true');
    alert('OTP verified (demo). Profile created/updated.');
    renderApp();
  } else {
    alert('Wrong OTP (use 123456 or Auto-verify in demo).');
  }
};
$('#autoVerify').onclick = ()=> { $('#otpInput').value='123456'; $('#verifyOtp').click(); };
$('#closeOtp').onclick = ()=> { otpModal.classList.add('hidden'); otpModal.setAttribute('aria-hidden','true'); };

// ---------------- routing helpers ----------------
function route(page){ window.location.hash = page; renderApp(); }

// ---------------- initial seed & boot ----------------
seed();
if(!window.location.hash) window.location.hash = 'login';
renderApp();
window.addEventListener('hashchange', renderApp);

// ---------------- submit handlers via global for buttons ----------------
window.viewJob = viewJob;
window.attemptApply = attemptApply;

// ---------------- posting notifications when a job is posted (function used in renderPostJob) -------------
function notifyUsersForCategory(category, job){
  // This local function mirrors the one above (kept for clarity)
  const profiles = store.get(KEYS.PROFILES) || {};
  const notifs = store.get(KEYS.NOTIFS) || {};
  Object.values(profiles).forEach(profile => {
    if(!profile.categories || profile.categories.length===0) return;
    if(profile.categories.includes(category) && profile.subscription && profile.subscription.active){
      const arr = notifs[profile.id] || [];
      arr.unshift({ id:uid(), text:`New ${category} job: ${job.title} • ₹${job.pay}`, job_id: job.id, ts: Date.now(), read:false });
      notifs[profile.id] = arr;
    }
  });
  store.set(KEYS.NOTIFS, notifs);
}

// make notifyUsersForCategory global for renderPostJob to call
window.notifyUsersForCategory = notifyUsersForCategory;

// ---------------- convenience: route to jobs on load ----------------
if(window.location.hash==='') window.location.hash='jobs';
renderApp();

</script>
</body>
</html>