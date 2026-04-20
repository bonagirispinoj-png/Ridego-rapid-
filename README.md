<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>RideGo v5 — Smart Rides</title>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800;900&display=swap" rel="stylesheet">
<style>
:root {
  --primary: #FF6B35; --primary-dark: #e55a25; --secondary: #1A1A2E; --accent: #16213E;
  --green: #00C853; --green-dark: #00A344; --red: #FF3D57; --yellow: #FFD600;
  --purple: #7C4DFF; --blue: #2196F3; --bg: #F5F5F5; --card: #ffffff;
  --text: #1A1A2E; --muted: #888; --border: #E8E8E8; --shadow: 0 4px 20px rgba(0,0,0,0.08);
}
* { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
body { font-family: 'Nunito', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; overflow-x: hidden; }
.screen { display: none; }
.screen.active { display: block; }
.app-header { background: var(--secondary); color: #fff; padding: 14px 16px; display: flex; align-items: center; justify-content: space-between; position: sticky; top: 0; z-index: 100; }
.header-sub { font-size: 11px; opacity: 0.6; font-weight: 400; }
.card { background: var(--card); border-radius: 16px; padding: 16px; margin: 10px; box-shadow: var(--shadow); }
.card-title { font-size: 16px; font-weight: 800; margin-bottom: 12px; }
input, select, textarea { display: block; width: 100%; padding: 13px 14px; margin: 6px 0 12px; border: 1.5px solid var(--border); border-radius: 12px; font-size: 15px; font-family: 'Nunito', sans-serif; background: #fff; color: var(--text); transition: border-color 0.2s; }
input:focus, select:focus, textarea:focus { outline: none; border-color: var(--primary); }
label { font-size: 13px; font-weight: 700; color: var(--muted); text-transform: uppercase; letter-spacing: 0.5px; display: block; margin-bottom: 2px; }
textarea { resize: vertical; min-height: 70px; }
.btn { display: block; width: 100%; padding: 14px; margin: 6px 0; border: none; border-radius: 12px; font-size: 15px; font-weight: 800; cursor: pointer; font-family: 'Nunito', sans-serif; transition: all 0.2s; text-align: center; }
.btn:active { transform: scale(0.97); }
.btn-primary { background: var(--primary); color: #fff; }
.btn-secondary { background: var(--secondary); color: #fff; }
.btn-green { background: var(--green); color: #fff; }
.btn-red { background: var(--red); color: #fff; }
.btn-gray { background: #E8E8E8; color: var(--text); }
.btn-outline { background: transparent; border: 2px solid var(--primary); color: var(--primary); }
.btn-sm { padding: 8px 14px; font-size: 13px; margin: 4px; width: auto; display: inline-block; border-radius: 8px; }
.btn-purple { background: var(--purple); color: #fff; }
.btn-blue { background: var(--blue); color: #fff; }
#screen-landing { min-height: 100vh; background: linear-gradient(160deg, var(--secondary) 0%, var(--accent) 60%, #0F3460 100%); }
.landing-hero { padding: 50px 20px 30px; text-align: center; }
.landing-hero .logo { font-size: 42px; font-weight: 900; color: #fff; letter-spacing: -1px; }
.landing-hero .logo span { color: var(--primary); }
.landing-hero .tagline { color: rgba(255,255,255,0.65); font-size: 15px; margin-top: 8px; }
.landing-hero .hero-icon { font-size: 72px; margin: 20px 0; animation: float 3s ease-in-out infinite; }
@keyframes float { 0%,100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
.landing-cards { padding: 0 16px 30px; }
.role-card { background: rgba(255,255,255,0.08); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.15); border-radius: 20px; padding: 20px; margin: 12px 0; cursor: pointer; transition: all 0.2s; display: flex; align-items: center; gap: 16px; }
.role-card:active { transform: scale(0.97); background: rgba(255,255,255,0.15); }
.role-card .role-icon { font-size: 36px; }
.role-card .role-info h3 { color: #fff; font-size: 17px; font-weight: 800; }
.role-card .role-info p { color: rgba(255,255,255,0.55); font-size: 13px; margin-top: 2px; }
.role-card .arrow { margin-left: auto; color: rgba(255,255,255,0.4); font-size: 20px; }
.map-container { height: 240px; border-radius: 14px; overflow: hidden; margin: 10px; background: #e8e8e8; position: relative; }
.map-placeholder { width:100%; height:100%; display:flex; align-items:center; justify-content:center; background:linear-gradient(135deg,#e3f2fd,#bbdefb); flex-direction:column; gap:8px; }
.map-placeholder .map-ph-icon { font-size:48px; }
.map-placeholder p { font-size:13px; color:#1565c0; font-weight:700; }
.locate-btn { position: absolute; bottom: 10px; right: 10px; background: white; border: none; border-radius: 50%; width: 40px; height: 40px; font-size: 18px; cursor: pointer; box-shadow: 0 2px 8px rgba(0,0,0,0.2); z-index: 500; }
.vehicle-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin: 10px 0; }
.vehicle-option { background: #f8f8f8; border: 2px solid transparent; border-radius: 12px; padding: 12px 6px; text-align: center; cursor: pointer; transition: all 0.2s; }
.vehicle-option.selected { border-color: var(--primary); background: #fff3ee; }
.vehicle-option .v-icon { font-size: 26px; display: block; }
.vehicle-option .v-name { font-size: 12px; font-weight: 700; margin-top: 4px; }
.vehicle-option .v-price { font-size: 11px; color: var(--muted); }
.fare-box { background: linear-gradient(135deg, #fff3ee, #ffe8dc); border: 1.5px solid #ffcdb5; border-radius: 14px; padding: 14px; margin: 8px 0; }
.fare-box .fare-amount { font-size: 28px; font-weight: 900; color: var(--primary); }
.fare-box .fare-label { font-size: 13px; color: var(--muted); }
.ride-tracking { text-align: center; padding: 20px 10px; }
.tracking-steps { display: flex; flex-direction: column; gap: 0; margin: 20px 10px; }
.tracking-step { display: flex; align-items: flex-start; gap: 14px; padding: 12px 0; position: relative; }
.tracking-step:not(:last-child)::after { content: ''; position: absolute; left: 15px; top: 50px; width: 2px; height: calc(100% - 20px); background: var(--border); }
.step-dot { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 15px; flex-shrink: 0; }
.step-dot.done { background: var(--green); }
.step-dot.active { background: var(--primary); animation: pulse 1.5s infinite; }
.step-dot.pending { background: var(--border); }
@keyframes pulse { 0%,100% { box-shadow: 0 0 0 0 rgba(255,107,53,0.4); } 50% { box-shadow: 0 0 0 8px rgba(255,107,53,0); } }
.step-info h4 { font-size: 15px; font-weight: 700; }
.step-info p { font-size: 13px; color: var(--muted); margin-top: 2px; }
.driver-info-card { background: var(--secondary); color: #fff; border-radius: 20px; padding: 16px; margin: 10px; }
.driver-info-card .d-name { font-size: 18px; font-weight: 800; }
.driver-info-card .d-detail { font-size: 13px; opacity: 0.7; margin-top: 2px; }
.driver-info-card .d-rating { color: var(--yellow); font-size: 13px; margin-top: 4px; }
.driver-avatar { width: 56px; height: 56px; border-radius: 50%; background: rgba(255,255,255,0.15); font-size: 28px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.contact-row { display: flex; gap: 8px; margin-top: 12px; }
.contact-btn { flex: 1; background: rgba(255,255,255,0.12); border: none; color: #fff; border-radius: 10px; padding: 10px; font-size: 12px; font-weight: 700; font-family: 'Nunito', sans-serif; cursor: pointer; }
.badge { display: inline-block; padding: 4px 10px; border-radius: 20px; font-size: 12px; font-weight: 800; }
.badge-pending { background: #FFF3E0; color: #E65100; }
.badge-accepted { background: #E3F2FD; color: #1565C0; }
.badge-completed { background: #E8F5E9; color: #1B5E20; }
.badge-cancelled { background: #FFEBEE; color: #B71C1C; }
.badge-approved { background: #E8F5E9; color: #1B5E20; }
.badge-rejected { background: #FFEBEE; color: #B71C1C; }
.badge-review { background: #EDE7F6; color: #4A148C; }
.badge-online { background: #E8F5E9; color: #1B5E20; }
.badge-trial { background: #E3F2FD; color: #1565C0; }
.tabs { display: flex; gap: 6px; padding: 10px 10px 0; overflow-x: auto; }
.tabs::-webkit-scrollbar { display: none; }
.tab-btn { padding: 8px 16px; border-radius: 20px; border: none; font-size: 13px; font-weight: 700; font-family: 'Nunito', sans-serif; cursor: pointer; white-space: nowrap; transition: all 0.2s; background: #E8E8E8; color: var(--text); }
.tab-btn.active { background: var(--primary); color: #fff; }
.ride-card { background: #fff; border-radius: 14px; padding: 14px; margin: 8px 10px; box-shadow: var(--shadow); border-left: 4px solid var(--border); }
.ride-card.pending { border-left-color: var(--yellow); }
.ride-card.accepted { border-left-color: var(--primary); }
.ride-card.completed { border-left-color: var(--green); }
.ride-card.cancelled { border-left-color: var(--red); }
.ride-card h4 { font-size: 15px; font-weight: 800; margin-bottom: 6px; }
.ride-card p { font-size: 13px; color: var(--muted); margin: 2px 0; }
.ride-card .fare { font-size: 20px; font-weight: 900; color: var(--primary); }
.route-line { display: flex; flex-direction: column; gap: 4px; background: #f8f8f8; border-radius: 10px; padding: 10px 12px; margin: 8px 0; }
.route-line .pickup { font-size: 13px; font-weight: 700; color: var(--green-dark); }
.route-line .drop { font-size: 13px; font-weight: 700; color: var(--red); }
.duty-toggle { background: var(--secondary); border-radius: 16px; padding: 16px; margin: 10px; display: flex; align-items: center; justify-content: space-between; }
.duty-toggle .toggle-info h3 { color: #fff; font-size: 16px; font-weight: 800; }
.duty-toggle .toggle-info p { color: rgba(255,255,255,0.5); font-size: 12px; }
.toggle-switch { position: relative; width: 56px; height: 28px; }
.toggle-switch input { opacity: 0; width: 0; height: 0; }
.toggle-slider { position: absolute; top: 0; left: 0; right: 0; bottom: 0; background: #555; border-radius: 28px; transition: 0.3s; cursor: pointer; }
.toggle-slider:before { position: absolute; content: ''; width: 22px; height: 22px; top: 3px; left: 3px; background: white; border-radius: 50%; transition: 0.3s; }
input:checked + .toggle-slider { background: var(--green); }
input:checked + .toggle-slider:before { transform: translateX(28px); }
.earnings-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin: 10px; }
.earnings-item { background: #fff; border-radius: 14px; padding: 14px; box-shadow: var(--shadow); text-align: center; }
.earnings-item .amount { font-size: 22px; font-weight: 900; color: var(--primary); }
.earnings-item .label { font-size: 12px; color: var(--muted); font-weight: 600; }
.admin-stat-grid { display: grid; grid-template-columns: repeat(2,1fr); gap: 10px; margin: 10px; }
.stat-card { background: var(--card); border-radius: 14px; padding: 16px; box-shadow: var(--shadow); text-align: center; }
.stat-card .stat-num { font-size: 28px; font-weight: 900; color: var(--primary); }
.stat-card .stat-label { font-size: 12px; color: var(--muted); font-weight: 700; }
.kyc-section { background: #f8f8f8; border-radius: 12px; padding: 14px; margin: 10px 0; border: 2px dashed var(--border); }
.kyc-section.uploaded { border-color: var(--green); background: #f0fff4; border-style: solid; }
.kyc-section.verified { border-color: var(--green); background: #e8f5e9; border-style: solid; }
.kyc-section.rejected { border-color: var(--red); background: #ffebee; border-style: solid; }
.kyc-section h4 { font-size: 14px; font-weight: 800; margin-bottom: 8px; }
.kyc-section .upload-status { font-size: 12px; color: var(--muted); }
.spinner { display: inline-block; width: 20px; height: 20px; border: 3px solid rgba(255,255,255,0.3); border-top-color: #fff; border-radius: 50%; animation: spin 0.8s linear infinite; vertical-align: middle; }
@keyframes spin { to { transform: rotate(360deg); } }
#toast { position: fixed; bottom: 80px; left: 50%; transform: translateX(-50%) translateY(20px); background: rgba(26,26,46,0.95); color: #fff; padding: 12px 20px; border-radius: 12px; font-size: 14px; font-weight: 600; z-index: 9999; opacity: 0; transition: all 0.3s; white-space: nowrap; pointer-events: none; max-width: 90vw; text-align: center; }
#toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }
.searching-animation { text-align: center; padding: 40px 20px; }
.search-ring { width: 80px; height: 80px; border: 4px solid var(--border); border-top-color: var(--primary); border-radius: 50%; animation: spin 1.2s linear infinite; margin: 0 auto 16px; }
.bottom-nav { position: fixed; bottom: 0; left: 0; right: 0; background: #fff; border-top: 1px solid var(--border); display: flex; z-index: 99; box-shadow: 0 -4px 20px rgba(0,0,0,0.08); }
.nav-item { flex: 1; padding: 10px 4px; text-align: center; cursor: pointer; transition: color 0.2s; color: var(--muted); font-size: 10px; font-weight: 700; display: flex; flex-direction: column; align-items: center; gap: 2px; }
.nav-item.active { color: var(--primary); }
.nav-item .nav-icon { font-size: 22px; }
.page-with-nav { padding-bottom: 70px; }
.section-label { font-size: 11px; font-weight: 800; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; padding: 8px 16px 4px; }
.location-input-wrap { position: relative; }
.location-input-wrap input { padding-right: 44px; }
.location-input-wrap .loc-icon { position: absolute; right: 12px; top: 50%; transform: translateY(-50%); font-size: 20px; cursor: pointer; }
.payment-row { display: flex; gap: 8px; margin: 8px 0; }
.payment-opt { flex: 1; border: 2px solid var(--border); border-radius: 12px; padding: 12px 6px; text-align: center; cursor: pointer; transition: all 0.2s; }
.payment-opt.selected { border-color: var(--primary); background: #fff3ee; }
.payment-opt .p-icon { font-size: 22px; }
.payment-opt .p-label { font-size: 12px; font-weight: 700; margin-top: 4px; }
.sos-btn { background: var(--red); color: #fff; border: none; border-radius: 50%; width: 60px; height: 60px; font-size: 24px; cursor: pointer; animation: pulse-sos 2s infinite; }
@keyframes pulse-sos { 0%,100% { box-shadow: 0 0 0 0 rgba(255,61,87,0.4); } 50% { box-shadow: 0 0 0 12px rgba(255,61,87,0); } }
.modal-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.6); z-index: 500; display: flex; align-items: flex-end; }
.modal-sheet { background: #fff; border-radius: 24px 24px 0 0; padding: 24px 16px; width: 100%; max-height: 80vh; overflow-y: auto; animation: slideUp 0.3s ease; }
@keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
.modal-handle { width: 40px; height: 4px; background: var(--border); border-radius: 4px; margin: 0 auto 16px; }
.approval-banner { background: #FFF3E0; border: 1.5px solid #FFB300; border-radius: 12px; padding: 14px; margin: 10px; }
.approval-banner h4 { color: #E65100; font-size: 15px; font-weight: 800; }
.approval-banner p { color: #BF360C; font-size: 13px; margin-top: 4px; }
.trial-banner { background: linear-gradient(135deg,#e3f2fd,#bbdefb); border: 1.5px solid var(--blue); border-radius: 12px; padding: 14px; margin: 10px; }
.trial-banner h4 { color: #1565C0; font-size: 15px; font-weight: 800; }
.trial-banner p { color: #1976D2; font-size: 13px; margin-top: 4px; }
.free-badge { background: linear-gradient(135deg, #00C853, #00A344); color: #fff; border-radius: 12px; padding: 12px 16px; margin: 10px; display: flex; align-items: center; gap: 12px; }
.free-badge .fb-icon { font-size: 28px; }
.free-badge h4 { font-size: 15px; font-weight: 800; }
.free-badge p { font-size: 12px; opacity: 0.9; margin-top: 2px; }
.pickup-mode-row { display: flex; gap: 8px; margin: 0 0 8px; }
.pickup-mode-btn { flex: 1; padding: 8px; border-radius: 10px; border: 2px solid var(--border); background: #f8f8f8; font-family: 'Nunito', sans-serif; font-size: 12px; font-weight: 700; cursor: pointer; text-align: center; transition: all 0.2s; }
.pickup-mode-btn.active { border-color: var(--primary); background: #fff3ee; color: var(--primary); }
.notif-banner { background: linear-gradient(135deg, var(--purple), #5c35cc); color: #fff; border-radius: 12px; padding: 12px 16px; margin: 10px; display: flex; align-items: center; justify-content: space-between; gap: 12px; }
.notif-banner p { font-size: 13px; font-weight: 700; flex: 1; }
.notif-banner button { background: #fff; color: var(--purple); border: none; border-radius: 8px; padding: 6px 12px; font-size: 12px; font-weight: 800; font-family: 'Nunito', sans-serif; cursor: pointer; white-space: nowrap; }
.live-dot { display: inline-block; width: 8px; height: 8px; background: var(--green); border-radius: 50%; animation: pulse-dot 1.5s infinite; margin-right: 5px; }
@keyframes pulse-dot { 0%,100% { box-shadow: 0 0 0 0 rgba(0,200,83,0.5); } 50% { box-shadow: 0 0 0 6px rgba(0,200,83,0); } }
.route-info-bar { background: linear-gradient(135deg,#fff3ee,#ffe8dc); border-radius: 12px; padding: 10px 14px; margin: 4px 10px; display: flex; align-items: center; justify-content: space-between; border: 1px solid #ffcdb5; }
.route-info-bar .ri-item { text-align: center; }
.route-info-bar .ri-val { font-size: 18px; font-weight: 900; color: var(--primary); }
.route-info-bar .ri-lbl { font-size: 10px; color: var(--muted); font-weight: 700; text-transform: uppercase; }
.chat-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.7); z-index: 600; display: flex; align-items: flex-end; }
.chat-sheet { background: #fff; border-radius: 24px 24px 0 0; width: 100%; height: 70vh; display: flex; flex-direction: column; animation: slideUp 0.3s ease; }
.chat-header { padding: 16px; border-bottom: 1px solid var(--border); display: flex; align-items: center; justify-content: space-between; }
.chat-messages { flex: 1; overflow-y: auto; padding: 12px 16px; display: flex; flex-direction: column; gap: 8px; }
.chat-msg { max-width: 75%; padding: 10px 14px; border-radius: 16px; font-size: 14px; }
.chat-msg.sent { background: var(--primary); color: #fff; border-radius: 16px 16px 4px 16px; align-self: flex-end; }
.chat-msg.received { background: #f0f0f0; color: var(--text); border-radius: 16px 16px 16px 4px; align-self: flex-start; }
.chat-msg .msg-time { font-size: 10px; opacity: 0.7; margin-top: 4px; }
.chat-input-row { padding: 12px 16px; border-top: 1px solid var(--border); display: flex; gap: 8px; align-items: center; padding-bottom: max(12px, env(safe-area-inset-bottom)); }
.chat-input-row input { margin: 0; border-radius: 24px; background: #f5f5f5; }
.chat-send-btn { background: var(--primary); border: none; border-radius: 50%; width: 44px; height: 44px; color: #fff; font-size: 18px; cursor: pointer; flex-shrink: 0; }
.quick-msgs { display: flex; gap: 6px; overflow-x: auto; padding: 8px 16px; }
.quick-msgs::-webkit-scrollbar { display: none; }
.quick-chip { background: #f0f0f0; border: none; border-radius: 20px; padding: 6px 14px; font-size: 12px; font-weight: 700; cursor: pointer; font-family: 'Nunito', sans-serif; white-space: nowrap; }
.incoming-alert { background: linear-gradient(135deg, var(--green), var(--green-dark)); color: #fff; border-radius: 16px; padding: 16px; margin: 10px; animation: pulse-alert 1.5s infinite; }
@keyframes pulse-alert { 0%,100% { box-shadow: 0 0 0 0 rgba(0,200,83,0.5); } 50% { box-shadow: 0 0 0 12px rgba(0,200,83,0); } }
.incoming-alert h3 { font-size: 18px; font-weight: 900; }
.incoming-alert .timer-bar { background: rgba(255,255,255,0.3); border-radius: 10px; height: 6px; margin: 10px 0; overflow: hidden; }
.incoming-alert .timer-fill { background: #fff; height: 100%; border-radius: 10px; transition: width 1s linear; }
.doc-verify-row { display: flex; align-items: center; gap: 10px; padding: 12px; background: #f8f8f8; border-radius: 10px; margin: 6px 0; }
.doc-verify-row .doc-icon { font-size: 24px; }
.doc-verify-row .doc-info { flex: 1; }
.doc-verify-row .doc-info h5 { font-size: 14px; font-weight: 700; }
.doc-verify-row .doc-info p { font-size: 12px; color: var(--muted); }
.doc-deadline { background: #fff3e0; border: 1px solid #ffb300; border-radius: 10px; padding: 10px 14px; margin: 8px 0; font-size: 13px; color: #e65100; font-weight: 700; }
.doc-deadline.urgent { background: #ffebee; border-color: var(--red); color: var(--red); }
.ride-progress { background: #f0f0f0; border-radius: 10px; height: 8px; margin: 8px 0; overflow: hidden; }
.ride-progress-fill { background: linear-gradient(90deg, var(--primary), var(--yellow)); height: 100%; border-radius: 10px; transition: width 0.5s ease; }
.driver-eta-chip { background: var(--secondary); color: #fff; border-radius: 12px; padding: 8px 14px; font-size: 13px; font-weight: 700; display: inline-flex; align-items: center; gap: 6px; }
.suggestions-box { background: #fff; border: 1.5px solid var(--border); border-radius: 12px; margin-top: -8px; margin-bottom: 8px; overflow: hidden; box-shadow: var(--shadow); max-height: 200px; overflow-y: auto; }
.suggestion-item { padding: 12px 14px; cursor: pointer; border-bottom: 1px solid var(--border); font-size: 14px; display: flex; align-items: center; gap: 8px; }
.suggestion-item:last-child { border-bottom: none; }
.suggestion-item:active { background: #f5f5f5; }
/* QR Payment Modal */
.qr-modal { position:fixed; inset:0; background:rgba(0,0,0,0.8); z-index:700; display:flex; align-items:flex-end; }
.qr-sheet { background:#fff; border-radius:24px 24px 0 0; width:100%; padding:24px 20px; animation:slideUp 0.3s ease; }
.qr-sheet h2 { font-size:20px; font-weight:900; text-align:center; margin-bottom:4px; }
.qr-sheet p { font-size:13px; color:var(--muted); text-align:center; margin-bottom:16px; }
.qr-img-wrap { text-align:center; }
.qr-img-wrap img { width:200px; height:200px; border-radius:16px; border:3px solid var(--primary); }
.qr-upi-id { background:#f8f8f8; border-radius:12px; padding:12px 16px; margin:12px 0; text-align:center; font-size:15px; font-weight:800; color:var(--secondary); }
/* Platform fee warning */
.fee-warning { background:linear-gradient(135deg,#fff3e0,#ffe0b2); border:1.5px solid #ff9800; border-radius:12px; padding:12px 14px; margin:8px 10px; }
.fee-warning h4 { color:#e65100; font-size:14px; font-weight:800; }
.fee-warning p { color:#bf360c; font-size:12px; margin-top:4px; }

/* v5 NEW STYLES */
.btn-back{background:rgba(255,255,255,.15);color:#fff;border:none;border-radius:10px;padding:8px 14px;font-size:14px;font-weight:800;cursor:pointer;font-family:'Nunito',sans-serif}
.btn-back:active{transform:scale(.97)}
.phone-strip{background:rgba(0,200,83,.07);border:1.5px solid rgba(0,200,83,.3);border-radius:12px;padding:10px 14px;margin:8px 0;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:8px}
.ph-label{font-size:11px;font-weight:800;color:var(--green-dark);display:block}
.phone-pill{display:inline-flex;align-items:center;gap:4px;background:rgba(0,200,83,.13);border:1.5px solid var(--green);border-radius:20px;padding:5px 11px;color:var(--green-dark);font-size:13px;font-weight:800;cursor:pointer;margin:2px}
.phone-pill:active{transform:scale(.96)}
.driver-found-banner{background:linear-gradient(135deg,#00C853,#00A344);color:#fff;border-radius:14px;padding:12px 14px;margin:8px 10px}
.feature-row{display:flex;gap:8px;overflow-x:auto;padding:8px 10px 4px}.feature-row::-webkit-scrollbar{display:none}
.feature-chip{flex-shrink:0;background:#fff;border:1.5px solid var(--border);border-radius:14px;padding:10px 12px;text-align:center;cursor:pointer;min-width:68px;box-shadow:var(--shadow)}
.feature-chip:active{background:var(--primary);color:#fff;border-color:var(--primary)}
.feature-chip .fc-icon{font-size:20px}.feature-chip .fc-label{font-size:10px;font-weight:700;color:var(--muted);margin-top:2px}
.dest-row{display:flex;gap:8px;overflow-x:auto;padding:4px 0 8px;margin-top:4px}.dest-row::-webkit-scrollbar{display:none}
.dest-chip{flex-shrink:0;background:#fff;border:1.5px solid var(--border);border-radius:12px;padding:7px 12px;font-size:12px;font-weight:700;cursor:pointer;white-space:nowrap;box-shadow:var(--shadow)}
.dest-chip:active{background:var(--primary);color:#fff;border-color:var(--primary)}
.star-row{display:flex;justify-content:center;gap:8px;margin:12px 0}
.star-row span{font-size:32px;cursor:pointer}
.ai-modal-base{position:fixed;inset:0;background:rgba(0,0,0,.6);z-index:800;display:flex;align-items:flex-end}
.ai-modal-inner{background:#fff;border-radius:24px 24px 0 0;width:100%;padding:24px 16px;max-height:85vh;overflow-y:auto;animation:slideUp .3s ease}
.ai-modal-handle{width:40px;height:4px;background:#e0e0e0;border-radius:4px;margin:0 auto 14px}
/* Leaflet map fallback */
#lmap-booking, #lmap-active, #lmap-driver { width:100%; height:100%; }
</style>
</head>
<body>
<div id="toast"></div>

<!-- ============================== LANDING ============================== -->
<div id="screen-landing" class="screen active">
  <div class="landing-hero">
    <div class="logo">Ride<span>Go</span></div>
    <div class="tagline">Fast · Safe · Reliable rides at your doorstep</div>
    <div class="hero-icon">🚀</div>
  </div>
  <div class="landing-cards">
    <div class="role-card" onclick="App.goUserBooking()">
      <div class="role-icon">🙋</div>
      <div class="role-info"><h3>Book a Ride</h3><p>Find bikes, autos, cars instantly</p></div>
      <div class="arrow">›</div>
    </div>
    <div class="role-card" onclick="App.showScreen('screen-driver-home')">
      <div class="role-icon">🏍️</div>
      <div class="role-info"><h3>I'm a Driver</h3><p>Register free, start earning today</p></div>
      <div class="arrow">›</div>
    </div>
    <div class="role-card" onclick="App.showAdminLogin()">
      <div class="role-icon">🛠</div>
      <div class="role-info"><h3>Admin Panel</h3><p>Manage rides, drivers &amp; payments</p></div>
      <div class="arrow">›</div>
    </div>
  </div>
</div>

<!-- ============================== USER OFFERS ==============================-->
<div id="screen-user-offers" class="screen page-with-nav">
  <div class="app-header"><div style="display:flex;align-items:center;gap:10px"><button class="btn-back" onclick="App.goUserBooking()">←</button><div style="font-size:18px;font-weight:800;color:#fff">Offers &amp; Rewards</div></div></div>
  <div id="offers-list" style="padding-top:8px"></div>
</div>

<!-- ============================== DRIVER HOME ============================== -->
<div id="screen-driver-home" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Driver Portal</div>
    <button class="btn btn-gray btn-sm" onclick="App.showScreen('screen-landing')">← Back</button>
  </div>
  <div style="padding:16px">
    <div style="background:linear-gradient(135deg,var(--primary),var(--primary-dark));border-radius:20px;padding:24px;color:#fff;margin-bottom:16px">
      <div style="font-size:32px;margin-bottom:8px">🏍️</div>
      <h2 style="font-size:22px;font-weight:900">Start Earning with RideGo</h2>
      <p style="opacity:0.85;font-size:14px;margin-top:6px">No upfront fees. Register free, start immediately. Documents not required — Admin can approve you without documents.</p>
    </div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:16px">
      <div style="background:#fff;border-radius:14px;padding:14px;box-shadow:var(--shadow);text-align:center"><div style="font-size:24px">💰</div><div style="font-weight:800;margin-top:4px">₹1,200/day</div><div style="font-size:12px;color:var(--muted)">Avg Earnings</div></div>
      <div style="background:#fff;border-radius:14px;padding:14px;box-shadow:var(--shadow);text-align:center"><div style="font-size:24px">⭐</div><div style="font-weight:800;margin-top:4px">20% Cut</div><div style="font-size:12px;color:var(--muted)">Platform Fee</div></div>
      <div style="background:#fff;border-radius:14px;padding:14px;box-shadow:var(--shadow);text-align:center"><div style="font-size:24px">📅</div><div style="font-weight:800;margin-top:4px">7 Days</div><div style="font-size:12px;color:var(--muted)">Trial Period</div></div>
      <div style="background:#fff;border-radius:14px;padding:14px;box-shadow:var(--shadow);text-align:center"><div style="font-size:24px">🆓</div><div style="font-weight:800;margin-top:4px">Free Join</div><div style="font-size:12px;color:var(--muted)">No Entry Fee</div></div>
    </div>
    <button class="btn btn-green" onclick="App.showScreen('screen-driver-signup')">🚀 Register as Driver — Free</button>
    <button class="btn btn-outline" onclick="App.showScreen('screen-driver-login')">Already registered? Login</button>
  </div>
</div>

<!-- ============================== DRIVER LOGIN ============================== -->
<div id="screen-driver-login" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Driver Login</div>
    <button class="btn btn-gray btn-sm" onclick="App.showScreen('screen-driver-home')">← Back</button>
  </div>
  <div class="card" style="margin-top:16px">
    <div class="card-title">🏍️ Welcome Back, Driver</div>
    <label>Email</label>
    <input id="d-login-email" type="email" placeholder="driver@email.com">
    <label>Password</label>
    <input id="d-login-pass" type="password" placeholder="••••••••">
    <button class="btn btn-primary" onclick="App.driverLogin()">Login</button>
    <button class="btn btn-outline" onclick="App.showScreen('screen-driver-signup')">New Driver? Register Here</button>
    <div style="text-align:center;margin-top:8px"><span style="font-size:13px;color:var(--muted);cursor:pointer" onclick="App.forgotPassword('driver')">Forgot password?</span></div>
  </div>
</div>

<!-- ============================== DRIVER SIGNUP ============================== -->
<div id="screen-driver-signup" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Driver Registration</div>
    <button class="btn btn-gray btn-sm" onclick="App.showScreen('screen-driver-home')">← Back</button>
  </div>
  <div style="background:linear-gradient(135deg,#00C853,#00A344);padding:14px 16px;color:#fff">
    <p style="font-size:15px;font-weight:800">⚡ Start Driving Instantly — 7-Day Free Trial!</p>
    <p style="font-size:13px;opacity:0.9;margin-top:4px">Register now and start earning TODAY. Documents optional — Admin can approve without documents!</p>
  </div>
  <div class="card">
    <div class="card-title">Personal Details</div>
    <label>Full Name</label>
    <input id="s-name" placeholder="As on Aadhaar card">
    <label>Email</label>
    <input id="s-email" type="email" placeholder="your@email.com">
    <label>Phone Number</label>
    <input id="s-phone" type="tel" placeholder="+91 XXXXX XXXXX">
    <label>Password</label>
    <input id="s-pass" type="password" placeholder="Min 8 characters">
    <label>Vehicle Type</label>
    <select id="s-vehicle">
      <option value="">Select vehicle type</option>
      <option>Bike</option><option>Auto</option><option>Car</option>
    </select>
    <label>Vehicle Number</label>
    <input id="s-vnum" placeholder="e.g. TS09AB1234">
    <label>UPI ID (for payouts)</label>
    <input id="s-upi" placeholder="name@upi or phone@bank">
    <label>Bank Account Number</label>
    <input id="s-bank-acc" placeholder="Account number (optional)">
    <label>IFSC Code</label>
    <input id="s-ifsc" placeholder="e.g. SBIN0001234 (optional)">
  </div>
  <div class="card">
    <div class="card-title">📄 KYC Documents <span style="background:#e3f2fd;color:#1565c0;font-size:11px;font-weight:800;padding:3px 8px;border-radius:8px;margin-left:6px">Optional — Upload within 7 days</span></div>
    <p style="font-size:13px;color:var(--muted);margin-bottom:12px">Admin can approve you <b>without documents</b>. You can also upload later.</p>
    <div class="kyc-section" id="kyc-licence"><h4>🪪 Driving Licence</h4>
      <input type="file" id="file-licence" accept="image/*,application/pdf" onchange="App.handleFileUpload('licence', this)">
      <p class="upload-status" id="status-licence">Not uploaded (optional)</p></div>
    <div class="kyc-section" id="kyc-rc"><h4>📋 Vehicle RC</h4>
      <input type="file" id="file-rc" accept="image/*,application/pdf" onchange="App.handleFileUpload('rc', this)">
      <p class="upload-status" id="status-rc">Not uploaded (optional)</p></div>
    <div class="kyc-section" id="kyc-aadhaar"><h4>🪪 Aadhaar Card</h4>
      <input type="file" id="file-aadhaar" accept="image/*,application/pdf" onchange="App.handleFileUpload('aadhaar', this)">
      <p class="upload-status" id="status-aadhaar">Not uploaded (optional)</p></div>
    <div class="kyc-section" id="kyc-bank"><h4>🏦 Bank Passbook / Cancel Cheque</h4>
      <input type="file" id="file-bank" accept="image/*,application/pdf" onchange="App.handleFileUpload('bank', this)">
      <p class="upload-status" id="status-bank">Not uploaded (optional)</p></div>
    <div class="kyc-section" id="kyc-insurance"><h4>🛡️ Vehicle Insurance</h4>
      <input type="file" id="file-insurance" accept="image/*,application/pdf" onchange="App.handleFileUpload('insurance', this)">
      <p class="upload-status" id="status-insurance">Optional</p></div>
    <div class="kyc-section" id="kyc-selfie"><h4>🤳 Profile Selfie</h4>
      <input type="file" id="file-selfie" accept="image/*" capture="user" onchange="App.handleFileUpload('selfie', this)">
      <p class="upload-status" id="status-selfie">Recommended</p></div>
  </div>
  <div style="padding:0 10px 16px">
    <button class="btn btn-green" id="signup-btn" onclick="App.driverSignup()">🚀 Register &amp; Start Driving Now →</button>
    <p style="font-size:12px;color:var(--muted);text-align:center;margin-top:8px">By registering, you agree to RideGo's Terms &amp; Privacy Policy</p>
  </div>
</div>

<!-- ============================== ADMIN LOGIN ============================== -->
<div id="screen-admin-login" class="screen">
  <div class="app-header">
    <div style="font-size:18px;font-weight:800;color:#fff">Admin Login</div>
    <button class="btn btn-gray btn-sm" onclick="App.showScreen('screen-landing')">← Back</button>
  </div>
  <div class="card" style="margin-top:16px">
    <div class="card-title">🛠 Admin Access</div>
    <label>Admin Email</label>
    <input id="a-login-email" type="email" placeholder="admin email" value="bonagirispinoj@gmail.com">
    <label>Password</label>
    <input id="a-login-pass" type="password" placeholder="••••••••">
    <button class="btn btn-secondary" onclick="App.adminLogin()" id="admin-login-btn">Login as Admin</button>
    <div style="text-align:center;margin-top:8px"><span style="font-size:13px;color:var(--muted);cursor:pointer" onclick="App.forgotPassword('admin')">Forgot password?</span></div>
  </div>
</div>

<!-- ============================== USER BOOKING ============================== -->
<div id="screen-user-booking" class="screen page-with-nav">
  <div class="app-header">
    <div style="display:flex;align-items:center;gap:10px">
      <button class="btn btn-gray btn-sm" style="margin:0;padding:6px 10px;font-size:18px;background:rgba(255,255,255,0.15);color:#fff;border:none" onclick="App.showScreen('screen-landing')">←</button>
      <div>
        <div style="font-size:18px;font-weight:800;color:#fff">Book Ride</div>
        <div class="header-sub" id="user-location-sub">Getting your location...</div>
      </div>
    </div>
    <button class="sos-btn" style="width:40px;height:40px;font-size:14px;border-radius:8px" onclick="App.triggerSOS()">SOS</button>
  </div>
  <div id="notif-banner" class="notif-banner" style="display:none">
    <p>🔔 Allow notifications for real-time ride updates</p>
    <button onclick="App.requestNotifPermission()">Allow</button>
  </div>
  <!-- AI Features Row -->
  <div class="feature-row">
    <div class="feature-chip" onclick="App.aiSuggest()"><div class="fc-icon">🤖</div><div class="fc-label">AI Pick</div></div>
    <div class="feature-chip" onclick="App.fareCompare()"><div class="fc-icon">💰</div><div class="fc-label">Compare</div></div>
    <div class="feature-chip" onclick="App.safetyInfo()"><div class="fc-icon">🛡️</div><div class="fc-label">Safety</div></div>
    <div class="feature-chip" onclick="App.rideTips()"><div class="fc-icon">💡</div><div class="fc-label">Tips</div></div>
    <div class="feature-chip" onclick="App.shareTrip()"><div class="fc-icon">📤</div><div class="fc-label">Share</div></div>
  </div>
  <!-- Map container (Leaflet) -->
  <div class="map-container" id="booking-map-container" style="height:260px">
    <div id="lmap-booking"></div>
    <button class="locate-btn" onclick="App.recenterMap()">🎯</button>
  </div>
  <!-- Route info bar -->
  <div id="route-info-bar" style="display:none" class="route-info-bar">
    <div class="ri-item"><div class="ri-val" id="ri-km">—</div><div class="ri-lbl">Distance</div></div>
    <div class="ri-item"><div class="ri-val" id="ri-time">—</div><div class="ri-lbl">Est. Time</div></div>
    <div class="ri-item"><div class="ri-val" id="ri-fare">—</div><div class="ri-lbl">Fare</div></div>
  </div>
  <div class="card" style="margin-top:0">
    <label>📍 Pickup Location</label>
    <div class="pickup-mode-row">
      <button class="pickup-mode-btn active" id="pm-live" onclick="App.setPickupMode('live')">📡 Live GPS</button>
      <button class="pickup-mode-btn" id="pm-manual" onclick="App.setPickupMode('manual')">✏️ Manual</button>
    </div>
    <div class="location-input-wrap">
      <input id="pickup-input" placeholder="Your pickup address" oninput="App.pickupInputChanged()">
      <span class="loc-icon" onclick="App.recenterMap()" title="Refresh location">🔄</span>
      <span class="loc-icon" onclick="App.recenterMap()">🎯</span>
    </div>
    <div id="pickup-suggestions" class="suggestions-box" style="display:none"></div>
    <label>🏁 Drop Location</label>
    <div class="location-input-wrap">
      <input id="drop-input" placeholder="Where are you going?" oninput="App.dropInputChanged()">
      <span class="loc-icon" onclick="App.clearDrop()" title="Clear drop">🔄</span>
    </div>
    <div id="drop-suggestions" class="suggestions-box" style="display:none"></div>
    <div class="dest-row">
      <div class="dest-chip" onclick="App.quickDest('Home')">🏠 Home</div>
      <div class="dest-chip" onclick="App.quickDest('Work')">🏢 Work</div>
      <div class="dest-chip" onclick="App.quickDest('Airport')">✈️ Airport</div>
      <div class="dest-chip" onclick="App.quickDest('Hospital')">🏥 Hospital</div>
      <div class="dest-chip" onclick="App.quickDest('Railway Station')">🚉 Station</div>
    </div>
  </div>
  <div class="section-label">Choose Vehicle</div>
  <div style="padding:0 10px">
    <div class="vehicle-grid" id="vehicle-grid">
      <div class="vehicle-option selected" onclick="App.selectVehicle(this,'Bike')">
        <span class="v-icon">🏍️</span><div class="v-name">Bike</div><div class="v-price">₹7/km · Base ₹15</div>
      </div>
      <div class="vehicle-option" onclick="App.selectVehicle(this,'Auto')">
        <span class="v-icon">🛺</span><div class="v-name">Auto</div><div class="v-price">₹15/km · Base ₹18</div>
      </div>
      <div class="vehicle-option" onclick="App.selectVehicle(this,'Car')">
        <span class="v-icon">🚗</span><div class="v-name">Car</div><div class="v-price">₹20/km · Base ₹25</div>
      </div>
    </div>
  </div>
  <div class="card" id="fare-preview">
    <div class="fare-box">
      <div class="fare-label">Estimated Fare</div>
      <div class="fare-amount" id="fare-display">₹—</div>
      <div style="font-size:12px;color:var(--muted);margin-top:4px" id="dist-display">Enter pickup &amp; drop to see estimate</div>
    </div>
    <label>Payment Method</label>
    <div class="payment-row">
      <div class="payment-opt selected" id="pay-cash" onclick="App.selectPayment('cash')">
        <div class="p-icon">💵</div><div class="p-label">Cash</div>
      </div>
      <div class="payment-opt" id="pay-upi" onclick="App.selectPayment('upi')">
        <div class="p-icon">📱</div><div class="p-label">UPI/Scan</div>
      </div>
      <div class="payment-opt" id="pay-wallet" onclick="App.selectPayment('wallet')">
        <div class="p-icon">💳</div><div class="p-label">Wallet</div>
      </div>
    </div>
    <div id="upi-qr-section" style="display:none">
      <div style="background:#f0fff4;border:1.5px solid var(--green);border-radius:12px;padding:12px;text-align:center;margin-top:8px">
        <p style="font-size:13px;font-weight:700;color:var(--green-dark)">📱 After booking, scan QR to pay driver directly</p>
        <button class="btn btn-green btn-sm" style="margin-top:6px" onclick="App.showPaymentQR(0)">Show Payment QR</button>
      </div>
    </div>
  </div>
  <div class="card">
    <div class="card-title">Your Details</div>
    <label>Your Name</label>
    <input id="user-name-input" placeholder="Full name">
    <label>Phone Number</label>
    <input id="user-phone-input" type="tel" placeholder="10-digit phone number">
    <label>Note for Driver (Optional)</label>
    <textarea id="user-note" placeholder="e.g. Call on arrival, gate number..." style="min-height:50px"></textarea>
    <button class="btn btn-primary" id="book-btn" onclick="App.bookRide()">🚀 Book Now</button>
  </div>
</div>

<!-- USER BOTTOM NAV -->
<div id="user-bottom-nav" class="bottom-nav" style="display:none">
  <div class="nav-item active" onclick="App.goUserBooking()" id="unav-book"><span class="nav-icon">🏠</span>Book</div>
  <div class="nav-item" onclick="App.showUserRides()" id="unav-rides"><span class="nav-icon">🕒</span>Rides</div>
  <div class="nav-item" onclick="App.showUserProfile()" id="unav-profile"><span class="nav-icon">👤</span>Profile</div>
</div>

<!-- ============================== SEARCHING ============================== -->
<div id="screen-searching" class="screen">
  <div class="app-header">
    <div style="display:flex;align-items:center;gap:10px"><button class="btn-back" onclick="App.cancelSearch()">← Back</button><div style="font-size:18px;font-weight:800;color:#fff">Finding Driver...</div></div>
  </div>
  <div class="searching-animation">
    <div class="search-ring"></div>
    <h3 id="search-status-text" style="font-size:17px;font-weight:800">🔍 Searching for nearby drivers...</h3>
    <p id="search-attempt-text" style="font-size:13px;color:var(--muted);margin-top:6px">This may take up to 60 seconds</p>
  </div>
  <div id="search-booking-summary"></div>
</div>

<!-- ============================== ACTIVE RIDE (USER) ============================== -->
<div id="screen-ride-active" class="screen">
  <div class="app-header">
    <div>
      <div id="active-ride-status-text" style="font-size:16px;font-weight:800;color:#fff">Driver on the way</div>
      <div class="header-sub">Track your ride live</div>
    </div>
    <button class="sos-btn" style="width:40px;height:40px;font-size:14px;border-radius:8px" onclick="App.triggerSOS()">SOS</button>
  </div>
  <div class="map-container" style="height:280px">
    <div id="lmap-active" style="width:100%;height:100%"></div>
  </div>
  <div id="driver-info-section"></div>
  <div class="tracking-steps" id="tracking-steps"></div>
  <div class="card" id="active-ride-actions"></div>
</div>

<!-- ============================== RIDE DONE ============================== -->
<div id="screen-ride-done" class="screen">
  <div class="app-header">
    <button class="btn btn-gray btn-sm" onclick="App.goUserBooking()">← Back</button>
  </div>
  <div style="text-align:center;padding:40px 20px 20px">
    <div style="font-size:60px">🎉</div>
    <h2 style="font-size:24px;font-weight:900;margin:12px 0">Ride Complete!</h2>
    <p style="color:var(--muted);font-size:15px">Thank you for riding with RideGo</p>
  </div>
  <div class="card"><div class="card-title">Ride Summary</div><div id="ride-done-summary"></div></div>
  <div class="card">
    <div class="card-title">⭐ Rate Your Driver</div>
    <div class="star-row" id="star-rating"><span onclick="App.setRating(1)">☆</span><span onclick="App.setRating(2)">☆</span><span onclick="App.setRating(3)">☆</span><span onclick="App.setRating(4)">☆</span><span onclick="App.setRating(5)">☆</span></div>
    <textarea id="ride-feedback" placeholder="Any feedback? (optional)"></textarea>
    <button class="btn btn-primary" onclick="App.submitRating()">Submit Rating</button>
    <button class="btn btn-gray" onclick="App.goUserBooking()">Skip &amp; Book Another Ride</button>
  </div>
</div>

<!-- ============================== USER RIDES ============================== -->
<div id="screen-user-rides" class="screen page-with-nav">
  <div class="app-header"><div style="display:flex;align-items:center;gap:10px"><button class="btn-back" onclick="App.goUserBooking()">←</button><div style="font-size:18px;font-weight:800;color:#fff">My Rides</div></div></div>
  <div id="user-rides-list" style="padding-top:8px"></div>
</div>

<!-- ============================== USER PROFILE ============================== -->
<div id="screen-user-profile" class="screen page-with-nav">
  <div class="app-header"><div style="display:flex;align-items:center;gap:10px"><button class="btn-back" onclick="App.goUserBooking()">←</button><div style="font-size:18px;font-weight:800;color:#fff">My Profile</div></div></div>
  <div class="card" style="margin-top:16px;text-align:center">
    <div style="font-size:56px">👤</div>
    <h3 id="profile-name" style="margin-top:8px;font-size:18px;font-weight:800">Guest User</h3>
  </div>
  <div class="card">
    <div class="card-title">Save Your Details</div>
    <label>Name</label><input id="profile-name-input" placeholder="Your name">
    <label>Phone</label><input id="profile-phone-input" type="tel" placeholder="Phone number">
    <button class="btn btn-primary" onclick="App.saveUserProfile()">Save Profile</button>
  </div>
  <div class="card"><div class="card-title">🏠 Saved Addresses</div>
    <label>Home</label><input id="saved-home" placeholder="Home address">
    <label>Work</label><input id="saved-work" placeholder="Work address">
    <button class="btn btn-outline" onclick="App.saveAddresses()">Save Addresses</button>
  </div>
  <div class="card">
    <div class="card-title">📞 Emergency Contact</div>
    <label>Name</label><input id="emer-name" placeholder="Emergency contact name">
    <label>Phone</label><input id="emer-phone" type="tel" placeholder="Emergency phone">
    <button class="btn btn-outline" onclick="App.saveEmergencyContact()">Save Emergency Contact</button>
  </div>
  <div style="padding:10px">
    <button class="btn btn-gray" onclick="App.goUserBooking()">← Back to Booking</button>
  </div>
</div>

<!-- ============================== DRIVER DASHBOARD ============================== -->
<div id="screen-driver" class="screen page-with-nav">
  <div class="app-header">
    <div>
      <div style="font-size:18px;font-weight:800;color:#fff" id="driver-header-name">Driver Dashboard</div>
      <div class="header-sub" id="driver-header-status">Loading...</div>
    </div>
    <button class="btn btn-gray btn-sm" onclick="App.driverLogout()">Logout</button>
  </div>
  <div id="driver-approval-banner" style="display:none"></div>
  <div id="driver-fee-warning" style="display:none"></div>
  <div class="duty-toggle" id="duty-toggle-section" style="display:none">
    <div class="toggle-info">
      <h3 id="duty-label">Go Online</h3>
      <p id="duty-sublabel">Tap to start accepting rides</p>
    </div>
    <label class="toggle-switch">
      <input type="checkbox" id="duty-toggle" onchange="App.toggleDuty(this.checked)">
      <span class="toggle-slider"></span>
    </label>
  </div>
  <div id="driver-main-content"></div>
</div>

<!-- DRIVER BOTTOM NAV -->
<div id="driver-bottom-nav" class="bottom-nav" style="display:none">
  <div class="nav-item active" onclick="App.driverNav('rides')" id="dnav-rides"><span class="nav-icon">🏠</span>Rides</div>
  <div class="nav-item" onclick="App.driverNav('earnings')" id="dnav-earnings"><span class="nav-icon">💰</span>Earn</div>
  <div class="nav-item" onclick="App.driverNav('kyc')" id="dnav-kyc"><span class="nav-icon">📄</span>Docs</div>
  <div class="nav-item" onclick="App.driverNav('withdraw')" id="dnav-withdraw"><span class="nav-icon">🏧</span>Withdraw</div>
  <div class="nav-item" onclick="App.driverNav('profile')" id="dnav-profile"><span class="nav-icon">👤</span>Profile</div>
</div>

<!-- ============================== DRIVER ACTIVE RIDE ============================== -->
<div id="screen-driver-ride" class="screen">
  <div class="app-header">
    <div>
      <div style="font-size:18px;font-weight:800;color:#fff" id="dride-status-title">Ride in Progress</div>
      <div class="header-sub" id="dride-status-sub">Heading to pickup</div>
    </div>
    <button class="sos-btn" style="width:40px;height:40px;font-size:14px;border-radius:8px" onclick="App.triggerSOS()">SOS</button>
  </div>
  <div class="map-container" style="height:260px">
    <div id="lmap-driver" style="width:100%;height:100%"></div>
  </div>
  <div id="driver-ride-detail"></div>
  <div class="card" id="driver-ride-actions"></div>
</div>

<!-- ============================== ADMIN DASHBOARD ============================== -->
<div id="screen-admin" class="screen page-with-nav">
  <div class="app-header">
    <div>
      <div style="font-size:18px;font-weight:800;color:#fff">Admin Panel</div>
      <div class="header-sub">RideGo Management</div>
    </div>
    <button class="btn btn-gray btn-sm" onclick="App.adminLogout()">Logout</button>
  </div>
  <div class="admin-stat-grid" id="admin-stats-grid"></div>
  <div class="tabs" id="admin-tabs">
    <button class="tab-btn active" onclick="App.adminTab('rides')">All Rides</button>
    <button class="tab-btn" onclick="App.adminTab('kyc')">🔍 KYC</button>
    <button class="tab-btn" onclick="App.adminTab('drivers')">Drivers</button>
    <button class="tab-btn" onclick="App.adminTab('withdrawals')">🏧 Payouts</button>
    <button class="tab-btn" onclick="App.adminTab('fees')">💸 Fees</button>
    <button class="tab-btn" onclick="App.adminTab('analytics')">📊 Stats</button>
  </div>
  <div id="admin-content" style="padding-bottom:70px"></div>
</div>
<div id="admin-bottom-nav" class="bottom-nav" style="display:none">
  <div class="nav-item active" onclick="App.adminTab('rides')"><span class="nav-icon">🚗</span>Rides</div>
  <div class="nav-item" onclick="App.adminTab('kyc')"><span class="nav-icon">🔍</span>KYC</div>
  <div class="nav-item" onclick="App.adminTab('withdrawals')"><span class="nav-icon">🏧</span>Payouts</div>
  <div class="nav-item" onclick="App.adminTab('fees')"><span class="nav-icon">💸</span>Fees</div>
  <div class="nav-item" onclick="App.adminTab('analytics')"><span class="nav-icon">📊</span>Stats</div>
</div>

<!-- ============================== CHAT OVERLAY ============================== -->
<div id="chat-overlay" style="display:none" class="chat-overlay" onclick="App.closeChatIfBg(event)">
  <div class="chat-sheet">
    <div class="modal-handle"></div>
    <div class="chat-header">
      <div>
        <div style="font-size:16px;font-weight:800" id="chat-title">Chat</div>
        <div style="font-size:12px;color:var(--muted)" id="chat-subtitle">End-to-end encrypted</div>
      </div>
      <button onclick="App.closeChat()" style="background:none;border:none;font-size:22px;cursor:pointer">✕</button>
    </div>
    <div class="quick-msgs" id="quick-msgs">
      <button class="quick-chip" onclick="App.sendQuick('I am 2 minutes away')">⏱ 2 mins away</button>
      <button class="quick-chip" onclick="App.sendQuick('I have arrived, please come out')">📍 Arrived</button>
      <button class="quick-chip" onclick="App.sendQuick('Please share exact location')">🗺 Share location</button>
      <button class="quick-chip" onclick="App.sendQuick('On the way!')">🚀 On the way</button>
      <button class="quick-chip" onclick="App.sendQuick('Traffic ahead, may be delayed')">🚦 Traffic delay</button>
    </div>
    <div class="chat-messages" id="chat-messages"></div>
    <div class="chat-input-row">
      <input id="chat-input" placeholder="Type a message..." onkeypress="if(event.key==='Enter')App.sendChat()">
      <button class="chat-send-btn" onclick="App.sendChat()">➤</button>
    </div>
  </div>
</div>

<!-- ============================== QR PAYMENT MODAL ============================== -->
<div id="qr-modal" style="display:none" class="qr-modal" onclick="App.closeQRIfBg(event)">
  <div class="qr-sheet">
    <div class="modal-handle"></div>
    <h2>💳 Pay via UPI / QR</h2>
    <p>Scan below to pay the driver fare directly</p>
    <div class="qr-img-wrap">
      <img id="qr-modal-img" src="" alt="QR Code" style="width:200px;height:200px;border-radius:16px;border:3px solid var(--primary)">
    </div>
    <div class="qr-upi-id" id="qr-upi-display">bonagirispinoj-1@oksbi</div>
    <p style="font-size:13px;font-weight:700;color:var(--primary);text-align:center" id="qr-amount-display"></p>
    <div style="display:flex;gap:8px;margin-top:8px">
      <button class="btn btn-green" onclick="window.open('upi://pay?pa=bonagirispinoj-1@oksbi&pn=RideGo&am='+App.getQRAmount()+'&cu=INR','_blank')">📱 Open UPI App</button>
      <button class="btn btn-gray" style="width:auto;padding:14px 20px" onclick="App.closeQR()">Close</button>
    </div>
    <p style="font-size:11px;color:var(--muted);text-align:center;margin-top:8px">Pay via GPay, PhonePe, Paytm, BHIM or any UPI app</p>
  </div>
</div>


<!-- Leaflet CSS -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<!-- ============================== FIREBASE + APP LOGIC ============================== -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import {
  getFirestore, collection, addDoc, onSnapshot, updateDoc,
  doc, query, where, runTransaction, setDoc, getDoc, getDocs,
  orderBy, limit, serverTimestamp, deleteDoc, increment
} from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
import {
  getAuth, createUserWithEmailAndPassword,
  signInWithEmailAndPassword, signOut, sendPasswordResetEmail
} from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

// ─── FIREBASE CONFIG ─────────────────────────────────────────────────────────
const firebaseConfig = {
  apiKey: "AIzaSyBIebPXVjzVBZl380ZCELg2LUSCLr-6QrE",
  authDomain: "neurocoin-ntc.firebaseapp.com",
  databaseURL: "https://neurocoin-ntc-default-rtdb.firebaseio.com",
  projectId: "neurocoin-ntc",
  storageBucket: "neurocoin-ntc.firebasestorage.app",
  messagingSenderId: "1040732555800",
  appId: "1:1040732555800:web:55e63b5ff81781c6b1916d"
};
const fbApp = initializeApp(firebaseConfig);
const db    = getFirestore(fbApp);
const auth  = getAuth(fbApp);
const ADMIN_EMAIL = "bonagirispinoj@gmail.com";
const ADMIN_PHONE = "919110563922";
const ADMIN_UPI   = "bonagirispinoj-1@oksbi";
const PLATFORM_FEE_PCT = 0.20; // 20%
const PLATFORM_FEE_DEADLINE_HRS = 24;

// ─── VEHICLE RATES ────────────────────────────────────────────────────────────
const VEHICLE_RATES = {
  "Bike": { base: 15, perKm: 7, icon: "🏍️", commission: 0.20 },
  "Auto": { base: 18, perKm: 15, icon: "🛺",  commission: 0.20 },
  "Car":  { base: 25, perKm: 20, icon: "🚗",  commission: 0.20 }
};

// ─── STATE ────────────────────────────────────────────────────────────────────
let state = {
  currentUser: null, role: null,
  userLocation: { lat: null, lng: null, address: "" },
  dropLocation: null,
  selectedVehicle: { name: "Bike", ...VEHICLE_RATES["Bike"] },
  selectedPayment: "cash",
  currentRideId: null,
  activeRideSub: null,
  driverRidesSub: null,
  driverDuty: false,
  uploadedDocs: {},
  pendingRating: null,
  userProfile: JSON.parse(localStorage.getItem("ridego_profile") || "{}"),
  currentRating: 0,
  estimatedFare: 0,
  estimatedKm: 0,
  pickupMode: "live",
  chatRideId: null,
  chatRole: null,
  chatSub: null,
  driverActiveRide: null,
  currentQRAmount: 0
};

let locationWatcher=null,unsubAdmin=null,_deb=null,_cache={};

// ─── LEAFLET MAP INSTANCES ────────────────────────────────────────────────────
let bookingMap = null, bookingPickupMarker = null, bookingDropMarker = null, bookingPolyline = null;
let activeMap = null, activeDriverMarker = null, activePickupMarker = null;
let driverMap = null, driverRoutePolyline = null, driverPickupMarker2 = null;

function initLeafletBookingMap(lat, lng) {
  const center = [lat || 17.9689, lng || 79.5941];
  if (!bookingMap) {
    bookingMap = L.map('lmap-booking', { zoomControl: true, attributionControl: false }).setView(center, 14);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19 }).addTo(bookingMap);
  } else {
    bookingMap.setView(center, 14);
  }
  updateLeafletPickupMarker(center[0], center[1]);
  // Force resize
  setTimeout(() => { if(bookingMap) bookingMap.invalidateSize(); }, 300);
}

function updateLeafletPickupMarker(lat, lng) {
  if (!bookingMap) return;
  if (bookingPickupMarker) bookingMap.removeLayer(bookingPickupMarker);
  bookingPickupMarker = L.marker([lat, lng], {
    icon: L.divIcon({ className:'', html:'<div style="background:#FF6B35;width:16px;height:16px;border-radius:50%;border:3px solid #fff;box-shadow:0 2px 6px rgba(0,0,0,0.3)"></div>', iconSize:[16,16], iconAnchor:[8,8] })
  }).addTo(bookingMap).bindPopup('📍 Pickup');
}

function updateLeafletDropMarker(lat, lng) {
  if (!bookingMap) return;
  if (bookingDropMarker) bookingMap.removeLayer(bookingDropMarker);
  bookingDropMarker = L.marker([lat, lng], {
    icon: L.divIcon({ className:'', html:'<div style="background:#00C853;width:16px;height:16px;border-radius:50%;border:3px solid #fff;box-shadow:0 2px 6px rgba(0,0,0,0.3)"></div>', iconSize:[16,16], iconAnchor:[8,8] })
  }).addTo(bookingMap).bindPopup('🏁 Drop');
}

async function drawLeafletRoute(pLat, pLng, dLat, dLng) {
  if (!bookingMap) return;
  if (bookingPolyline) bookingMap.removeLayer(bookingPolyline);
  // INSTANT haversine shown immediately
  const hKm = haversineKm(pLat,pLng,dLat,dLng)*1.3, hMin = Math.ceil(hKm*2.5);
  updateLeafletPickupMarker(pLat,pLng); updateLeafletDropMarker(dLat,dLng);
  const v = state.selectedVehicle, hFare = Math.round(v.base+hKm*v.perKm);
  state.estimatedFare=hFare; state.estimatedKm=hKm;
  const fd=document.getElementById("fare-display"); if(fd) fd.textContent="~₹"+hFare;
  const dd=document.getElementById("dist-display"); if(dd) dd.textContent="~"+hKm.toFixed(1)+" km · ~"+hMin+" min";
  const bar=document.getElementById("route-info-bar"); if(bar) bar.style.display="flex";
  ["ri-km","ri-time","ri-fare"].forEach((id,i)=>{ const el=document.getElementById(id); if(el) el.textContent=[`~${hKm.toFixed(1)} km`,`~${hMin} min`,`₹${hFare}`][i]; });
  // OSRM in background
  try {
    const resp = await fetch(`https://router.project-osrm.org/route/v1/driving/${pLng},${pLat};${dLng},${dLat}?overview=full&geometries=geojson`,{signal:AbortSignal.timeout(5000)});
    const data = await resp.json();
    if (data.routes && data.routes[0]) {
      const km=(data.routes[0].distance/1000), mins=Math.ceil(data.routes[0].duration/60), fare=Math.round(v.base+km*v.perKm);
      state.estimatedFare=fare; state.estimatedKm=km;
      const coords=data.routes[0].geometry.coordinates.map(c=>[c[1],c[0]]);
      if(bookingPolyline) bookingMap.removeLayer(bookingPolyline);
      bookingPolyline=L.polyline(coords,{color:"#FF6B35",weight:5,opacity:.85}).addTo(bookingMap);
      bookingMap.fitBounds(bookingPolyline.getBounds(),{padding:[30,30]});
      if(fd) fd.textContent="₹"+fare; if(dd) dd.textContent=km.toFixed(1)+" km · "+mins+" min";
      ["ri-km","ri-time","ri-fare"].forEach((id,i)=>{ const el=document.getElementById(id); if(el) el.textContent=[`${km.toFixed(1)} km`,`${mins} min`,`₹${fare}`][i]; });
      return {km,mins,distText:km.toFixed(1)+" km",timeText:mins+" min"};
    }
  } catch(_) {}
  return {km:hKm,mins:hMin,distText:"~"+hKm.toFixed(1)+" km",timeText:"~"+hMin+" min"};
}

function initActiveMap(lat, lng) {
  if (!activeMap) {
    activeMap = L.map('lmap-active', { zoomControl: false, attributionControl: false }).setView([lat, lng], 15);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19 }).addTo(activeMap);
  } else {
    activeMap.setView([lat, lng], 15);
  }
  setTimeout(() => { if(activeMap) activeMap.invalidateSize(); }, 300);
}

function updateActiveMapDriver(dLat, dLng, pLat, pLng) {
  if (!activeMap) { initActiveMap(dLat || 17.9689, dLng || 79.5941); }
  if (activeDriverMarker) activeMap.removeLayer(activeDriverMarker);
  activeDriverMarker = L.marker([dLat, dLng], {
    icon: L.divIcon({ className:'', html:'<div style="background:var(--yellow);width:20px;height:20px;border-radius:50%;border:3px solid #fff;box-shadow:0 2px 8px rgba(0,0,0,0.4);font-size:12px;display:flex;align-items:center;justify-content:center">🏍</div>', iconSize:[20,20], iconAnchor:[10,10] })
  }).addTo(activeMap);
  if (pLat && !activePickupMarker) {
    activePickupMarker = L.marker([pLat, pLng], {
      icon: L.divIcon({ className:'', html:'<div style="background:#2196F3;width:16px;height:16px;border-radius:50%;border:3px solid #fff;box-shadow:0 2px 6px rgba(0,0,0,0.3)"></div>', iconSize:[16,16], iconAnchor:[8,8] })
    }).addTo(activeMap).bindPopup('📍 Your Pickup');
  }
  activeMap.panTo([dLat, dLng]);
}

function initDriverMap(lat, lng) {
  if (!driverMap) {
    driverMap = L.map('lmap-driver', { zoomControl: true, attributionControl: false }).setView([lat, lng], 14);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { maxZoom: 19 }).addTo(driverMap);
  }
  setTimeout(() => { if(driverMap) driverMap.invalidateSize(); }, 300);
}

async function showDriverRouteOnMap(pLat, pLng, dLat, dLng) {
  if (!driverMap) { initDriverMap(dLat || 17.9689, dLng || 79.5941); }
  if (driverRoutePolyline) driverMap.removeLayer(driverRoutePolyline);
  if (driverPickupMarker2) driverMap.removeLayer(driverPickupMarker2);
  try {
    const url = `https://router.project-osrm.org/route/v1/driving/${dLng},${dLat};${pLng},${pLat}?overview=full&geometries=geojson`;
    const resp = await fetch(url);
    const data = await resp.json();
    if (data.routes && data.routes[0]) {
      const coords = data.routes[0].geometry.coordinates.map(c => [c[1], c[0]]);
      driverRoutePolyline = L.polyline(coords, { color: '#00C853', weight: 5 }).addTo(driverMap);
      driverMap.fitBounds(driverRoutePolyline.getBounds(), { padding: [30, 30] });
    }
  } catch(e) {}
  driverPickupMarker2 = L.marker([pLat, pLng], {
    icon: L.divIcon({ className:'', html:'<div style="background:#FF6B35;width:18px;height:18px;border-radius:50%;border:3px solid #fff;box-shadow:0 2px 6px rgba(0,0,0,0.3)"></div>', iconSize:[18,18], iconAnchor:[9,9] })
  }).addTo(driverMap).bindPopup('📍 User Pickup');
}

// ─── HAVERSINE ───────────────────────────────────────────────────────────────
function haversineKm(lat1, lng1, lat2, lng2) {
  const R = 6371, dLat = (lat2-lat1)*Math.PI/180, dLng = (lng2-lng1)*Math.PI/180;
  const a = Math.sin(dLat/2)**2 + Math.cos(lat1*Math.PI/180)*Math.cos(lat2*Math.PI/180)*Math.sin(dLng/2)**2;
  return R*2*Math.atan2(Math.sqrt(a),Math.sqrt(1-a));
}

// ─── CALC ROUTE & FARE ────────────────────────────────────────────────────────
async function calcRouteAndFare() {
  if (!state.userLocation.lat || !state.dropLocation?.lat) return;
  const v = state.selectedVehicle;
  document.getElementById("fare-display").textContent = "Calculating...";
  const result = await drawLeafletRoute(state.userLocation.lat, state.userLocation.lng, state.dropLocation.lat, state.dropLocation.lng);
  const km = result.km;
  const fare = Math.round(v.base + km * v.perKm);
  state.estimatedFare = fare;
  state.estimatedKm = km;
  document.getElementById("fare-display").textContent = `₹${fare}`;
  document.getElementById("dist-display").textContent = `${result.distText} · ${result.timeText} · Base ₹${v.base}+₹${Math.round(km*v.perKm)}`;
  const bar = document.getElementById("route-info-bar");
  bar.style.display = "flex";
  document.getElementById("ri-km").textContent = result.distText;
  document.getElementById("ri-time").textContent = result.timeText;
  document.getElementById("ri-fare").textContent = `₹${fare}`;
  updateLeafletPickupMarker(state.userLocation.lat, state.userLocation.lng);
  updateLeafletDropMarker(state.dropLocation.lat, state.dropLocation.lng);
}

// ─── GPS ──────────────────────────────────────────────────────────────────────
function startGPS() {
  if (locationWatcher !== null) return;
  if (!navigator.geolocation) { toast("⚠️ GPS not supported"); return; }
  locationWatcher = navigator.geolocation.watchPosition(
    pos => {
      state.userLocation.lat = pos.coords.latitude;
      state.userLocation.lng = pos.coords.longitude;
      if (state.pickupMode === "live") {
        reverseGeocode(pos.coords.latitude, pos.coords.longitude);
        if (bookingMap) { bookingMap.panTo([pos.coords.latitude, pos.coords.longitude]); updateLeafletPickupMarker(pos.coords.latitude, pos.coords.longitude); }
      }
      if (state.role === "driver" && state.driverDuty && state.currentUser) {
        updateDoc(doc(db, "users", state.currentUser.uid), {
          lat: pos.coords.latitude, lng: pos.coords.longitude
        }).catch(() => {});
        // Also update active ride if driving
        if (state.driverActiveRide) {
          updateDoc(doc(db, "rides", state.driverActiveRide), {
            driverLat: pos.coords.latitude, driverLng: pos.coords.longitude
          }).catch(() => {});
        }
      }
    },
    err => {
      const el = document.getElementById("user-location-sub");
      if (el) el.textContent = "Enable GPS for accurate location";
    },
    { enableHighAccuracy: true, maximumAge: 8000, timeout: 15000 }
  );
}
startGPS();

async function reverseGeocode(lat, lng) {
  try {
    const r = await fetch(`https://nominatim.openstreetmap.org/reverse?lat=${lat}&lon=${lng}&format=json`);
    const d = await r.json();
    state.userLocation.address = d.display_name || `${lat.toFixed(4)}, ${lng.toFixed(4)}`;
    const el = document.getElementById("pickup-input");
    if (el && state.pickupMode === "live") el.value = state.userLocation.address;
    const sub = document.getElementById("user-location-sub");
    if (sub) sub.textContent = "📡 " + (d.address?.city || d.address?.town || d.address?.village || "Location found");
  } catch (e) {}
}

// ─── TOAST ────────────────────────────────────────────────────────────────────
function toast(msg, duration=3000) {
  const t = document.getElementById("toast");
  t.textContent = msg; t.classList.add("show");
  clearTimeout(toast._t);
  toast._t = setTimeout(() => t.classList.remove("show"), duration);
}

// ─── SCREEN MANAGEMENT ────────────────────────────────────────────────────────
function showScreen(id) {
  document.querySelectorAll(".screen").forEach(s => s.classList.remove("active"));
  const el = document.getElementById(id);
  if (el) { el.classList.add("active"); window.scrollTo(0,0); }
  ["user-bottom-nav","driver-bottom-nav","admin-bottom-nav"].forEach(n => {
    const nav = document.getElementById(n);
    if (nav) nav.style.display = "none";
  });
  if (["screen-user-booking","screen-user-rides","screen-user-profile"].includes(id)) {
    document.getElementById("user-bottom-nav").style.display = "flex";
  } else if (id === "screen-driver") {
    document.getElementById("driver-bottom-nav").style.display = "flex";
  } else if (id === "screen-admin") {
    document.getElementById("admin-bottom-nav").style.display = "flex";
  }
  // Invalidate maps on show
  setTimeout(() => {
    if (id === "screen-user-booking" && bookingMap) bookingMap.invalidateSize();
    if (id === "screen-ride-active" && activeMap) activeMap.invalidateSize();
    if (id === "screen-driver-ride" && driverMap) driverMap.invalidateSize();
  }, 200);
}

function requestNotifPermission() {
  if (!("Notification" in window)) return;
  Notification.requestPermission().then(p => {
    const b = document.getElementById("notif-banner");
    if (b) b.style.display = "none";
    if (p === "granted") toast("🔔 Notifications enabled!");
  });
}
function sendLocalNotif(title, body) {
  if (Notification.permission === "granted") new Notification(title, { body, icon: "🚀" });
}

// ─── PLACE AUTOCOMPLETE (Nominatim) ──────────────────────────────────────────
async function fetchPlaceSuggestions(val){
  if(_cache[val])return _cache[val];
  try{const r=await fetch(`https://nominatim.openstreetmap.org/search?q=${encodeURIComponent(val)}&format=json&limit=5&countrycodes=in`,{signal:AbortSignal.timeout(4000)});const d=await r.json();_cache[val]=d;return d;}catch(_){return[];}
}

// ─── PLATFORM FEE CHECK ────────────────────────────────────────────────────────
async function checkDriverPendingFees() {
  if (!state.currentUser) return false;
  try {
    const q = query(collection(db,"platformFees"),
      where("driverUid","==",state.currentUser.uid),
      where("paid","==",false));
    const snap = await getDocs(q);
    let overdue = false;
    snap.forEach(d => {
      const fee = d.data();
      const deadline = new Date(fee.dueBy);
      if (new Date() > deadline) { overdue = true; }
    });
    return overdue;
  } catch(e) { return false; }
}

// QR base64 data (embedded)
const QR_IMAGE_B64 = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABaAAAAfQCAYAAAA0HfggAACAAElEQVR4XuzdZ5Rk92Gmdx37g44/rL3HXtGSddZnbR+H1VISRcpippfLJQGKIAESDAAJEiYIigQRB3kGA2Bynp7YYTrn7uqcqqu6u2JXzjl0nA7Tk2cwGEQG+XVVExSlaWhFUrzUAHg+/M6tdKtuQA26nrr1v7/3yus/EwAAAAAAAAAAv22/d+MNAAAAAAAAAAD8NhCgAQAAAAAAAACGIEADAAAAAAAAAAxBgAYAAAAAAAAAGIIADQAAAAAAAAAwBAEaAAAAAAAAAGAIAjQAAAAAAAAAwBAEaAAAAAAAAACAIQjQAAAAAAAAAABDEKABAAAAAAAAAIYgQAMAAAAAAAAADEGABgAAAAAAAAAYggANAAAAAAAAADAEARoAAAAAAAAAYAgCNAAAAAAAAADAEARoAAAAAAAAAIAhCNAAAAAAAAAAAEMQoAEAAAAAAAAAhiBAAwAAAAAAAAAMQYAGAAAAAAAAABiCAA0AAAAAAAAAMAQBGgAAAAAAAABgCAI0AAAAAAAAAMAQBGgAAAAAAAAAgCEI0AAAAAAAAAAAQxCgAQAAAAAAAACGIEADAAAAAAAAAAxBgAYAAAAAAAAAGIIADQAAAAAAAAAwBAEaAAAAAAAAAGAIAjQAAAAAAAAAwBAEaAAAAAAAAACAIQjQAAAAAAAAAABDEKABAAAAAAAAAIYgQAMAAAAAAAAADEGABgAAAAAAAAAYggANAAAAAAAAADAEARoAAAAAAAAAYAgCNAAAAAAAAADAEARoAAAAAAAAAIAhCNAAAAAAAAAAAEMQoAEAAAAAAAAAhiBAAwAAAAAAAAAMQYAGAAAAAAAAABiCAA0AAAAAAAAAMAQBGgAAAAAAAABgCAI0AAAAAAAAAMAQBGgAAAAAAAAAgCEI0AAAAAAAAAAAQxCgAQAAAAAAAACGIEADAAAAAAAAAAxBgAYAAAAAAAAAGIIADQAAAAAAAAAwBAEaAAAAAAAAAGAIAjQAAAAAAAAAwBAEaAAAAAAAAACAIQjQAAAAAAAAAABDEKABAAAAAAAAAIYgQAMAAAAAAAAADEGABgAAAAAAAAAYggANAAAAAAAAADAEARoAAAAAAAAAYAgCNAAAAAAAAADAEARoAAAAAAAAAIAhCNAAAAAAAAAAAEMQoAEAAAAAAAAAhiBAAwAAAAAAAAAMQYAGAAAAAAAAABiCAA0AAAAAAAAAMAQBGgAAAAAAAABgCAI0AAAAAAAAAMAQBGgAAAAAAAAAgCEI0AAAAAAAAAAAQxCgAQAAAAAAAACGIEADAAAAAAAAAAxBgAYAAAAAAAAAGIIADQAAAAAAAAAwBAEaAAAAAAAAAGAIAjQAAAAAAAAAwBAEaAAAAAAAAACAIQjQAAAAAAAAAABDEKABAAAAAAAAAIYgQAMAAAAAAAAADEGABgAAAAAAAAAYggANAAAAAAAAADAEARoAAAAAAAAAYAgCNAAAAAAAAADAEARoAAAAAAAAAIAhCNAAAAAAAAAAAEMQoAEAAAAAAAAAhiBAAwAAAAAAAAAMQYAGAAAAAAAAABiCAA0AAAAAAAAAMAQBGgAAAAAAAABgCAI0AAAAAAAAAMAQBGgAAAAAAAAAgCEI0AAAAAAAAAAAQxCgAQAAAAAAAACGIEADAAAAAAAAAAxBgAYAAAAAAAAAGIIADQAAAAAAAAAwBAEaAAAAAAAAAGAIAjQAAAAAAAAAwBAEaAAAAAAAAACAIQjQAAAAAAAAAABDEKABAAAAAAAAAIYgQAMAAAAAAAAADEGABgAAAAAAAAAYggANAAAAAAAAADAEARoAAAAAAAAAYAgCNAAAAAAAAADAEARoAAAAAAAAAIAhCNAAAAAAAAAAAEMQoAEAAAAAAAAAhiBAAwAAAAAAAAAMQYAGAAAAAAAAABiCAA0AAAAAAAAAMAQBGgAAAAAAAABgCAI0AAAAAAAAAMAQBGgAAAAAAAAAgCEI0AAAAAAAAAAAQxCgAQAAAAAAAACGIEADAAAAAAAAAAxBgAYAAAAAAAAAGIIADQAAAAAAAAAwBAEaAAAAAAAAAGAIAjQAAAAAAAAAwBAEaAAAAAAAAACAIQjQAAAAAAAAAABDEKABAAAAAAAAAIYgQAMAAAAAAAAADEGABgAAAAAAAAAYggANAAAAAAAAADAEARoAAAAAAAAAYAgCNAAAAAAAAADAEARoAAAAAAAAAIAhCNAAAAAAAAAAAEMQoAEAAAAAAAAAhiBAAwAAAAAAAAAMQYAGAAAAAAAAABiCAA0AAAAAAAAAMAQBGgAAAAAAAABgCAI0AAAAAAAAAMAQBGgAAAAAAAAAgCEI0AAAAAAAAAAAQxCgAQAAAAAAAACGIEAD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BGgA0HB7AiIWEqARAwIAzA8CtC4BWocAjTEkQOsSoHUI0LoEaADQcHsCIhYSoBEDAgDMDwK0LgFahwCNMSRA6xKgdQjQugRoANBwewIiFhKgEQOCz1dffWUmTpxoxo4dayfgsf3444/N5MmTzZw5c9yhozB37lwzZcoUM378eG9srI9yH8l1TnUfffjhh6ZPnz5m3333NWussUZ0JQDIQv2RRx5xh44GAVo3dYAeM2aMjVZ77bWXdw/EcP311zdnnHGGeeKJJ9yho5E6QMvDmKWXXtrep+7x4TzlPpUAPXToUO+9MIavvvqqufTSS82PfvQj7/rk4gYbbPDv14J7fDEcPny4ueiii8wPf/hDb+xY7rrrruZ3v/udGT16tDd+DAcMGGCOP/54s/rqq3tjx7ARAXrw4MH2c23dddf1XicxlM8beRgjc4AU/GuOKnMY9/pgfZQ1iFxnud5QP9yegIiFBGjEgODz7rvvmkGDBplrr73W7gCNrQSlF1980XzxxRfu0FGYNm2aGTFihLn33nu9sbE+3n333eaVV14xU6dOdW+BKHz++ed2EX399dfbqBHbrl27mjvvvNP87W9/c4eOBgFaN3WAll2TEsQkKrn3QAzPPfdc+1735ptvukNHI3WAln93u+22M4cffrh3fDhP2bUqAfqyyy7z3gtj2LNnT3PAAQeYDh06eNcnF+V9bvfdd7fvre7xxVDO/3777Zds97C44YYb2m/GpLrOJ598sn3Pa9eunTd2DBsRoOUz86677rKfPe7rJIbymS8PemQOkAKZs8jcReYw7vXB+njPPffYh1ZffvmlewtADXB7AiIWEqARA4LPsGHD7ELrxz/+sf35gdjK18XlK6apdpdMmjTJ3H///XaXjzs21sdOnTrZCf4nn3zi3gJRmDlzpt2h9Prrr5vnn38+ui+88IJ92CNffU8FAVo3dYCWYxg3bpyNJu49EEM59++9916yWCKkDtBrrrmmOeqoo+y1do8P59m/f39z4YUX2teC+14Yw6222sqstdZapk2bNt71ycXFF1/crLrqqmbTTTf1ji+GW265pb1XW7du7Y0dy7Zt29rAvfXWW3vjx3CjjTaynwff+973vLFj2IgALZ+Z8tkpGxnc10kM5TNfPvtlDpAC+ayRh4bHHnusd32wPh533HHmvvvus98ohfrh9gRELCRAIwYEnwcffND85je/MYsttpi3uIihLHRld48sIFIgCwfZwSIB3R0b66N8DVp2lf7jH/9wbwH4JwRo3dQBug6kDtDy0wndunUzr732mjs0/BN5gCEBWnbIuucPsSo2IkDnjmy+uOaaa8z222/vnT+sjzvuuKO57rrrzEcffeTeAlAD3J6AiIUEaMSA4EOAxhwkQOsQoHUJ0DoE6PIhQGMOEqB1CNDNIQG63rg9ARELCdCIAcGHAI05SIDWIUDrEqB1CNDlQ4DGHCRA6xCgm0MCdL1xewIiFhKgEQOCDwEac5AArUOA1iVA6xCgy4cAjTlIgNYhQDeHBOh64/YERCwkQCMGBB8CNOYgAVqHAK1LgNYhQJcPARpzkACtQ4BuDgnQ9cbtCYhYSIBGDAg+BGjMQQK0DgFalwCtQ4AuHwI05iABWocA3RwSoOuN2xMQsZAAjRgQfAjQmIMEaB0CtC4BWocAXT4EaMxBArQOAbo5JEDXG7cnIGIhARoxIPgQoDEHCdA6BGhdArQOAbp8CNCYgwRoHQJ0c0iArjduT0DEQgI0YkDwIUBjDhKgdQjQugRoHQJ0+RCgMQcJ0DoE6OaQAF1v3J6AiIUEaMSA4EOAxhwkQOsQoHUJ0DoE6PIhQGMOEqB1CNDNIQG63rg9ARELCdCIAcGHAI05SIDWIUDrEqB1CNDlQ4DGHCRA6xCgm0MCdL1xewIiFhKgEQOCDwEac5AArUOA1iVA6xCgy4cAjTlIgNYhQDeHBOh64/YERCwkQCMGBB8CNOYgAVqHAK1LgNYhQJcPARpzkACtQ4BuDgnQ9cbtCYhYSIBGDAg+BGjMQQK0DgFalwCtQ4AuHwI05iABWocA3RwSoOuN2xMQsZAAjRgQfAjQmIMEaB0CtC4BWocAXT4EaMxBArQOAbo5JEDXG7cnIGIhARoxIPgQoDEHCdA6BGhdArQOAbp8CNCYgwRoHQJ0c0iArjduT0DEQgI0YkDwIUBjDhKgdQjQugRoHQJ0+RCgMQcJ0DoE6OaQAF1v3J6AiIUEaMSA4EOAxhwkQOsQoHUJ0DoE6PIhQGMOEqB1CNDNIQG63rg9ARELCdCIAcGHAI05SIDWIUDrEqB1CNDlQ4DGHCRA6xCgm0MCdL1xewIiFhKgEQOCDwEac5AArUOA1iVA6xCgy4cAjTlIgNYhQDeHBOh64/YERCwkQCMGBB8CNOYgAVqHAK1LgNYhQJcPARpzkACtQ4BuDgnQ9cbtCYhYSIBGDAg+AwYMMEcccYRp3bq1WXTRRaO7zTbb2AD91ltvmdmzZ0dXguS1115rJ3/uhDAXF1poIdOiRQuzyCKLeOcP57nDDjvYAD1mzBjvHsjFuXPnmq+//tp9CUYjdYBea621zBlnnGGee+4579hiOHXqVNO3b1+z3377edc/ljvvvLMN0LJIdMfHeQ4ePNgcd9xxZoUVVvDOXww33nhjG6BHjBjhjZ2Lc+bMSfpabkSAXnjhhbP+zJH/uxyDfH66xxbL3M9RaldddVVz8skn28+eVMjrTD473ddgLP/1Wv7mm2/coaMwbty4f2+ScM9fLGX+KPeqe//GMvUcVf5d+fdTvpZTS4CuN25PQMRCAjRiQPB55ZVXzE033WQOP/xwc8ghh0RXQkbPnj3NH/7wB3P33XdHt3fv3jagr7/++t6EMBdlp+Fmm21mfv7zn3vnD+d5wgknmIsvvtjcdttt3j2Qg/fee6/561//mnQHd+oALUFy7733ttfBPb4Y3nnnnebKK680p556qnf9Y9m5c2dz6aWXmjvuuMMbH+cpi+izzz7bHHbYYd75i2GnTp1sXL355pu9sXNx6NCh5r333jOzZs1yX4ZRSB2gW7Zsab/R0LFjR+/65OLuu+9uz488PHePL4atWrUy6667rtlpp528sXGexx9/vLn99tvNm2++6d7C0ZAdxPLQUz5D3ddhDB999FH7/582bZo7dBQ+++wz8/DDD9vXs3v+YvjLX/7S7q5ebbXVkgXcdu3amS222MLss88+3vgxlLmvzIFTfeumERKg643bExCxkACNGBB8ZLIkX6n/y1/+Yh544IHo9unTx1xwwQU2cO+///7R3Wuvvcymm25qJ8juhDAXZZErsef3v/+9d/5wnhKrzj33XPtzMe49kIMHHXSQ/Rru888/774Eo5E6QC+xxBKmQ4cONsi4xxfDAw880Jx++un2Gw3u9Y+lPLCSuHrooYd64+M8TzzxRHP55ZfbOOOevxjKQ6Tu3bubo48+2hs7Fy+66CIzZMiQZNEqdYBecsklza677mo/m93rk4u9evWy1yLV+52E7d12283eq+7YOM+BAweakSNHmokTJ7q3cDReeOEF+9l58MEHe6/DGMrngfwUXaqfZZo+fbr9CTp5aOWev1jK56b8TFmqAL3eeuvZjR633HKLN3YMZe4rc2CZX7hj5yIBut64PQERCwnQiAGh8bz88st2gr/OOut4Ezacp/xMiez8/Pvf/+6ePvgnsntYdsbKV37d85eD8jVT2ekji61UpA7Qqf3e975nfv3rX9uHYal48sknbWBdfvnlvfFxnhIm5YHPpEmT3NMXhTfeeMP06NHDbLLJJt7YuSi7b+VbPV988YV7eFFIHaCXWWYZc+yxx9rdn7ny6quvmvPOO8/GMff4YrjccsvZb9488cQT7tDQQPr162cfGMquffcaxTD3+Zf8dMj9999vH3Kn+hmO1H+DQ879VVddZbbddltv7FwkQNcbtycgYiEBGjEgNB4CtG7uC6BGQIDWIUDrEKB1CdC6BOjyIUA3BwToMAToakiArjduT0DEQgI0YkBoPARo3dwXQI2AAK1DgNYhQOsSoHUJ0OVDgG4OCNBhCNDVkABdb9yegIiFBGjEgNB4CNC6uS+AGgEBWocArUOA1iVA6xKgy4cA3RwQoMMQoKshAbreuD0BEQsJ0IgBofEQoHVzXwA1AgK0DgFahwCtS4DWJUCXDwG6OSBAhyFAV0MCdL1xewIiFhKgEQNC4yFA6+a+AGoEBGgdArQOAVqXAK1LgC4fAnRzQIAOQ4CuhgToeuP2BEQsJEAjBoTGQ4DWzX0B1AgI0DoEaB0CtC4BWpcAXT4E6OaAAB2GAF0NCdD1xu0JiFhIgEYMCI2HAK2b+wKoERCgdQjQOgRoXQK0LgG6fAjQzQEBOgwBuhoSoOuN2xMQsZAAjRgQGg8BWjf3BVAjIEDrEKB1CNC6BGhdAnT5EKCbAwJ0GAJ0NSRA1xu3JyBiIQEaMSA0HgK0bu4LoEZAgNYhQOsQoHUJ0LoE6PIhQDcHBOgwBOhqSICuN25PQMRCAjRiQGg8BGjd3BdAjYAArUOA1iFA6xKgdQnQ5UOAbg4I0GEI0NWQAF1v3J6AiIUEaMSA0HgI0Lq5L4AaAQFahwCtQ4DWJUDrEqDLhwDdHBCgwxCgqyEBut64PQERCwnQiAGh8RCgdXNfADUCArQOAVqHAK1LgNYlQJcPAbo5IECHIUBXQwJ0vXF7AiIWEqARA0LjIUDr5r4AagQEaB0CtA4BWpcArUuALh8CdHNAgA5DgK6GBOh64/YERCwkQCMGhMZDgNbNfQHUCAjQOgRoHQK0LgFalwBdPgTo5oAAHYYAXQ0J0PXG7QmIWEiARgwIjYcArZv7AqgREKB1CNA6BGhdArQuAbp8CNDNAQE6DAG6GhKg643bExCxkACNGBAaDwFaN/cFUCMgQOsQoHUI0LoEaF0CdPkQoJsDAnQYAnQ1JEDXG7cnIGIhARoxIDQeArRu7gugRkCA1iFA6xCgdQnQugTo8iFANwcE6DAE6GpIgK43bk9AxEICNGJAaDwEaN3cF0CNgACtQ4DWIUDrEqB1CdDlQ4BuDgjQYQjQ1ZAAXW/cnoCIhQRoxIDQeAjQurkvgBoBAVqHAK1DgNYlQOsSoMuHAN0cEKDDEKCrIQG63rg9ARELCdCIAaHxEKB1c18ANQICtA4BWocArUuA1iVAlw8BujkgQIchQFdDAnS9cXsCIhYSoBEDQuMhQOvmvgBqBARoHQK0DgFalwCtS4AuHwJ0c0CADkOAroYE6Hrj9gRELCRAIwYEH4kMb731lnnmmWdswIptnz59zK9+9SvTvn17b8KG80y9AJo5c6YZN26cXbC71ycXb7rpJnPggQcmC4cSP1dZZRWz2Wab2YVEbDt27GjD5+9//3vv2GJ5/fXXm/3339+0a9fOO74YyjmSBwCbb765d3wx3GWXXcxFF11k34tSQYDW3Wqrrczpp59uHnroIe8ei6E8hJEALa9n9x6IocSSDh062MjqHlssCdA6n332mZ1bPPvss949EMP77rvPXHDBBWa//fbz7oEY/vznP7fRbfjw4e6hwT+ZNWuWDW4jR470rk8s5TOzc+fO5ic/+Yl3jWL429/+1txzzz1m/Pjx7uFlgQTooUOH2veLH//4x97xxVAeDF922WVmwIAB3vWJobyW5WHPBhts4L1X5eKmm25qNwD079/fO74clHnX6NGjkz14zh23JyBiIQEaMSD4yEK3b9++NjjI5Cm2Bx98sNlyyy1NmzZtvAkbzjN1gJZIIguUa665xrs+uSixSuLwkksu6Z2/GEqQ2Wmnncxpp51md7Gk8JJLLjHnnHOOd2yxlPgsi6AllljCO74YStiWSCzvFe6xxVAeMkgQe//9991bOBoEaN3VVlvNxp5OnTp591gM5f6RWCI73tx7IIZXXHGF/bbB+uuv7x1bLAnQOqNGjTJ33nmnOeOMM7x7IIby73bv3t306tXLuwdiKA/P5XPzww8/dA8N/smUKVNsvJLz5V6fWMpnpnx2utcnlhI/5SGDHEuuvPvuu+bhhx+2D6Hd44uhvKfKwx6ZH7nXJ4ZHHHGE2WGHHcyKK67ovVflomyykVh/1FFHeceXg3JtZS0onz3g4/YERCwkQCMGBJ/BgwebLl26mLXWWst+dT+28jVWiYaLLLKIN2HDeaYO0B9//LGNJfvss493fXJR4qeE1RYtWnjnL4ays/e4446zuz5lR1dsP/jgA3PbbbfZnUTuscUy9TlaffXV7S4lWei6xxdDuU8///xzM2PGDPcWjgYBWrdVq1b2gaHEAPcei+H2229vunXrZsOVew/EUHbdXn311Taiu8cWSwK0zpAhQ8wpp5xi1l13Xe8eiKF8q0S+MfHcc89590AMZUfs5MmT7TeIYP5MmDDB/PGPf7S70N3rE0v5Bp18do4dO9a7RjGcOHGi+fLLL83cufmuEaZPn24/O91ji+UjjzxiH/jIt5/c6xPDFVZYwbRu3TrZz6w0Qvm/y+emHIt7fDm45ppr2m8ayLUGH7cnIGIhARoxIPg8+OCD5je/+Y1ZbLHFvAkVNsbUAVp2cMnuZwk/7tg4zzXWWMMusJ5//nn39EVh9uzZ5u6777a7lN2xc3Httdc2Z511lnnppZfcw8sGAnT5ytesJUC/9tpr7uWJgkTDW2+91eyxxx7e2LEkQOsMHDjQ7mxM9a0V+UbMxRdfbB84QDnIQ0PZdSs//eBen1hK3L7rrrvsz31AOeT+NzhQtxF/gyNn3J6AiIUEaMSA4EOALl8CdPkSoHUJ0BhDArQOAVqXAF0+BOjmgABdfwnQYdyegIiFBGjEgOBDgC5fAnT5EqB1CdAYQwK0DgFalwBdPgTo5oAAXX8J0GHcnoCIhQRoxIDgQ4AuXwJ0+RKgdQnQGEMCtA4BWpcAXT4E6OaAAF1/CdBh3J6AiIUEaMSA4EOALl8CdPkSoHUJ0BhDArQOAVqXAF0+BOjmgABdfwnQYdyegIiFBGjEgOBDgC5fAnT5EqB1CdAYQwK0DgFalwBdPgTo5oAAXX8J0GHcnoCIhQRoxIDgQ4AuXwJ0+RKgdQnQGEMCtA4BWpcAXT4E6OaAAF1/CdBh3J6AiIUEaMSA4EOALl8CdPkSoHUJ0BhDArQOAVqXAF0+BOjmgABdfwnQYdyegIiFBGjEgOBDgC5fAnT5EqB1CdAYQwK0DgFalwBdPgTo5oAAXX8J0GHcnoCIhQRoxIDgQ4AuXwJ0+RKgdQnQGEMCtA4BWpcAXT4E6OaAAF1/CdBh3J6AiIUEaMSA4EOALl8CdPkSoHUJ0BhDArQOAVqXAF0+BOjmgABdfwnQYdyegIiFBGjEgOBDgC5fAnT5EqB1CdAYQwK0DgFalwBdPgTo5oAAXX8J0GHcnoCIhQRoxIDgQ4AuXwJ0+RKgdQnQGEMCtA4BWpcAXT4E6OaAAF1/CdBh3J6AiIUEaMSA4EOALl8CdPkSoHUJ0BhDArQOAVqXAF0+BOjmgABdfwnQYdyegIiFBGjEgOBDgC5fAnT5EqB1CdAYQwK0DgFalwBdPgTo5oAAXX8J0GHcnoCIhQRoxIDgQ4AuXwJ0+RKgdQnQGEMCtA4BWpcAXT4E6OaAAF1/CdBh3J6AiIUEaMSA4EOALl8CdPkSoHUJ0BhDArQOAVqXAF0+BOjmgABdfwnQYdyegIiFBGjEgOBDgC5fAnT5EqB1CdAYQwK0DgFalwBdPgTo5oAAXX8J0GHcnoCIhQRoxIDgQ4AuXwJ0+RKgdQnQGEMCtA4BWpcAXT4E6OaAAF1/CdBh3J6AiIUEaMSA4EOALl8CdPkSoHUJ0BhDArQOAVqXAF0+BOjmgABdfwnQYdyegIiFBGjEgOBDgC5fAnT5EqB1CdAYQwK0DgFalwBdPgTo5oAAXX8J0GHcnoCIhQRoxIDgkzpAt2rVyiy77LJmtdVWswErtquvvrpp165dsv9/I0wdoMePH29uv/12c9BBB3nnLxfbt29v2rRpYxZZZBHv/MVwlVVWMcccc4x54IEHzHvvvRfdt99+20axww8/3Du2WK688spJz5G8hiVayQLFPb4Yvv/++2bChAlm2rRp7i0cDXnAIGFv66239s5fDOUcSdyT9z33/OWivJcut9xy9qGMe3wx3G233ewDsdGjR7uXJwpTp041999/vznqqKO8sWN58MEHmxtvvNFGdPc+juGgQYNMly5dzLrrrutdnxjK+8Qvf/lLc8cdd3hjx1Le7+SB2+KLL+6NH8PUAXru3LlmypQp5qOPPvKOLZby2Sz36zfffOMOH4WZM2eaSZMmmQ8++MAbO4byMPLqq682v/jFL7zXSCwPO+ww+7BHrrM7PjZGeT898sgjzUorreS9DnNwoYUWsg/CVlxxRe/+ykWZW8haKtXcggAdxu0JiFhIgEYMCD6pA7RM+GR3jIQr2T0ZW9nNuMsuu2S9MyN1gJYdgc8884zp3bu3d/5yUSbGW221lVlqqaW88xfDtm3bmh133NHeT5dffnkSe/bsac4//3zv2GJ56KGHmi233DLZjkNZ/MhruXPnzt6xxbBXr17moYceMu+88457C0dDFtPynifXwj1/MezUqZO9j1ZYYQXv/OWihGeJxCeddJJ3fDGUYPXYY4/Z3ZMpmDFjhg1jf/zjH72xYymv4x49epjLLrvMu49jeMopp9jPtVT3kURheQhz9NFHe2PHUnY/y/tRy5YtvfFjmDpAf/XVV2bEiBHmnnvu8Y4tlv369TOjRo0yc+bMcYePggTup556ytxyyy3e2DGU+19eB+edd573GomlvNbk/dodGxvnCSecYHbYYQf74Mp9HebgwgsvbNZff337oMS9v3JR1lAy/0r1EIAAHcbtCYhYSIBGDAg+qQO0TPok6vXv399Ggdg+/PDD9qcTtthiC2/sXEwdoOWrq5988ol58803vfOXi7Kb7pBDDkkWZGRXiTwskfv1Bz/4QXS33XZbc/zxx5s+ffp4xxZLiQyyM1N2r7rHF0M5R7L4kZ9QcI8vhvITMRdccIENJqmQ3YZjx441r776qnf+Yig76GWxvt5663nnLxfl/eicc84xjz/+uHd8MXz99dfNuHHjzPTp093LEwXZuTpx4kT7rQN37FjKzuGTTz7ZPmxw7+MYbrTRRvYbDak+l+VbEvI+0aFDB2/sWMrubfl2ksQfd/wYpg7Qn3/+ufnzn/9s37fdY4vl6aefbucwslM5BfKZLzv19913X2/sGO688852/iU/keG+RmIpn5n/j73zgLKiShvtM42OOccZ5xezgChGjJgzpjGNMybMYcyKihjREXMaRQyDOgKmMSCKijmAJFGCguTQ5IyEBr7nV9DUvec0dRo8dc+t6r3X2uut/03bRd0693bVrnNP6THQv6Hm9rE06nmRnnuldTMpbfXz7vDDD5f77rvPGl9ZUcOw3vzX8y9z/3xIgE7G7AmIGEuARkwQbNIO0DprVWfJDBo0yNy0F0qxBmHaph2g80DW1yBcZZVVooCugTItPv/882jWqoYrc/tZMA8XQBo9W7ZsKbvuuqu1f1nxsMMOk6effjr66j5Uz/vvvx8t2bPuuutarx+WxrQD9Pjx4+XJJ5+Ugw8+2Nq2L48//nh58cUXU7sZ0717d2nWrJlss8021rZ9qDckdamYNG8a6ixx/XZPVuMnhrcU519po9/e0tnoekPA3D8f5uH8K03MnoCIsQRoxATBhgAdXgK0GwK0GwJ0eAjQtQMCdHgJ0G4I0IilOf9KGwJ0WMyegIixBGjEBMGGAB1eArQbArQbAnR4CNC1AwJ0eAnQbgjQiKU5/0obAnRYzJ6AiLEEaMQEwYYAHV4CtBsCtBsCdHgI0LUDAnR4CdBuCNCIpTn/ShsCdFjMnoCIsQRoxATBhgAdXgK0GwK0GwJ0eAjQtQMCdHgJ0G4I0IilOf9KGwJ0WMyegIixBGjEBMGGAB1eArQbArQbAnR4CNC1AwJ0eAnQbgjQiKU5/0obAnRYzJ6AiLEEaMQEwYYAHV4CtBsCtBsCdHgI0LUDAnR4CdBuCNCIpTn/ShsCdFjMnoCIsQRoxATBhgAdXgK0GwK0GwJ0eAjQtQMCdHgJ0G4I0IilOf9KGwJ0WMyegIixBGjEBMGGAB1eArQbArQbAnR4CNC1AwJ0eAnQbgjQiKU5/0obAnRYzJ6AiLEEaMQEwYYAHV4CtBsCtBsCdHgI0LUDAnR4CdBuCNCIpTn/ShsCdFjMnoCIsQRoxATBhgAdXgK0GwK0GwJ0eAjQtQMCdHgJ0G4I0IilOf9KGwJ0WMyegIixBGjEBMGGAB1eArQbArQbAnR4CNC1AwJ0eAnQbgjQiKU5/0obAnRYzJ6AiLEEaMQEwYYAHV4CtBsCtBsCdHgI0LUDAnR4CdBuCNCIpTn/ShsCdFjMnoCIsQRoxATBhgAdXgK0GwK0GwJ0eAjQtQMCdHgJ0G4I0IilOf9KGwJ0WMyegIixBGjEBMGGAB1eArQbArQbAnR4CNC1AwJ0eAnQbgjQiKU5/0obAnRYzJ6AiLEEaMQEwYYAHV4CtBsCtBsCdHgI0LUDAnR4CdBuCNCIpTn/ShsCdFjMnoCIsQRoxATBhgAdXgK0GwK0GwJ0eAjQtQMCdHgJ0G4I0IilOf9KGwJ0WMyegIixBGjEBMGGAB1eArQbArQbAnR4CNC1AwJ0eAnQbgjQiKU5/0obAnRYzJ6AiLEEaMQEwYYAHV4CtBsCtBsCdHgI0LUDAnR4CdBuCNCIpTn/ShsCdFjMnoCIsQRoxATBhgAdXgK0GwK0GwJ0eAjQtQMCdHgJ0G4I0IilOf9KGwJ0WMyegIixBGjEBMGGAB1eArQbArQbAnR4CNC1AwJ0eAnQbgjQiKU5/0obAnRYzJ6AiLEEaMQEwebdd9+V8847T9Zbb73oBMS3++yzj9x///3RyVMa5CFA6wnlPffcE11Iz54927tz5syRyspKWbBggfnyeWP+/Pkyb948a9u+/PTTT6ML3T/96U/W65cFS3EBlHaAXmGFFWTllVeOQoD5PvfhOuusI2eddVb0GpnHPyv27ds3CtB77rmntX++1LG04oorWsfHl2kH6IULF0afR/q5ZL5+WbFjx45ywQUXyCabbGIdHx+uuuqq0XtN33Pm8fGh/t6VVloptfeyqr9bt2Fu25d5CNDHHnusPPfcczJ58mRrjPlQb9xed911svXWW1vb9uGmm24ql1xyiXz44YfWtn356quvRmFs7bXXtsZYFqx6L6f5mY3J6t/MU089Vdq1a2eNL1/OnTs3Og/Wv29pQIAOi9kTEDGWAI2YINj07NlTnnrqKTn33HOjkw/f3nrrrfLOO+/I2LFjzU17IQ8BWi8ONU4+/PDD8tJLL3lXZxDpTCi9oE6L4cOHyxdffCEvv/yytX0ftmjRQo488kjZYIMNrNcvC+YhQK+11lpSt25dOeqoo6z3uQ81Pl9//fXy4IMPWsc/K7Zu3VruuOOOKMqY++fD008/XRo1aiRbbLGFdXx8mXaAnjp1qvzwww9RxDVfv6yon9U6s/Scc86xjpEPNUzWr18/uiljHh8fahzedttt5ZBDDrG27cuDDjpI6tSpE8U3c/s+zEOAbtiwoVx88cXy/PPPW2PMh3feeaccd9xx0Y0Sc9s+1G8AHH744XLLLbdY2/blAw88EP1d0G/qmWMsC5500knRNwE32mgj6/XD0qg3wvTvpp4fmePLl3oTRj+L0vo2AwE6LGZPQMRYAjRigmCjAbdHjx5RJNYTD9/qzNWBAwfKzJkzzU17IQ8BWmefa9jTC7kTTjjBu02bNpVnn31WBgwYYL58XtAZHzrTSme6n3jiidb2fXjAAQdEoT6tpWLSNg8BWme76fHV42y+z32or43GZ90H8/hnRY3oetNNZzWa++fD9u3bR6+PBg3z+Pgy7QA9YsQI6dChg1x55ZXW65cVL7vssmjZJN0P8xj58JFHHpHTTjsttSWH1lxzzejvzW233WZt25fNmzeP4q2GDXP7PsxDgNYlLHbbbbcoEptjzIeNGzeObjSsscYa1rZ9qMdWbzLst99+1rZ9qZ93Dz30UHQj3RxjWVDPvfQcbIcddrBePyyNOvtcvz2n30wyx5cvb7755uim6qRJk8yPEi8QoMNi9gREjCVAIyYI+SMPATpt9cT7qquukq+++sp8+bygAVq/JqtfceRrptWbhwCtNwBuvPFG+e6778xNe0G/xqozifRiztx2Vtxuu+2iC9FevXqZu+cFvZH3wgsvRDNkzW37Mu0A3a9fv2iW+M4772xtOyvqtzHatGkjU6ZMMXfPCzpDXOOw3pg0t+3D9ddfXy688ELp3LmzuWlvaIzRGeIau83t+zAPARrd6gzi//73v9ESB1lk5MiR0Tcm9t13X2vfMD/qJIlHH31URo8ebQ4BLxCgw2L2BESMJUAjJgj5gwDtlgAdXgK0GwK0GwJ0eUiAdkOARh8SoDELEqDzjdkTEDGWAI2YIOQPArRbAnR4CdBuCNBuCNDlIQHaDQEafUiAxixIgM43Zk9AxFgCNGKCkD8I0G4J0OElQLshQLshQJeHBGg3BGj0IQEasyABOt+YPQERYwnQiAlC/iBAuyVAh5cA7YYA7YYAXR4SoN0QoNGHBGjMggTofGP2BESMJUAjJgj5gwDtlgAdXgK0GwK0GwJ0eUiAdkOARh8SoDELEqDzjdkTEDGWAI2YIOQPArRbAnR4CdBuCNBuCNDlIQHaDQEafUiAxixIgM43Zk9AxFgCNGKCkD8I0G4J0OElQLshQLshQJeHBGg3BGj0IQEasyABOt+YPQERYwnQiAlC/iBAuyVAh5cA7YYA7YYAXR4SoN0QoNGHBGjMggTofGP2BESMJUAjJgj5gwDtlgAdXgK0GwK0GwJ0eUiAdkOARh8SoDELEqDzjdkTEDGWAI2YIOQPArRbAnR4CdBuCNBuCNDlIQHaDQEafUiAxixIgM43Zk9AxFgCNGKCkD8I0G4J0OElQLshQLshQJeHBGg3BGj0IQEasyABOt+YPQERYwnQiAlC/iBAuyVAh5cA7YYA7YYAXR4SoN0QoNGHBGjMggTofGP2BESMJUAjJgj5gwDtlgAdXgK0GwK0GwJ0eUiAdkOARh8SoDELEqDzjdkTEDGWAI2YIOQPArRbAnR4CdBuCNBuCNDlIQHaDQEafUiAxixIgM43Zk9AxFgCNGKCkD8I0G4J0OElQLshQLshQJeHBGg3BGj0IQEasyABOt+YPQERYwnQiAlC/iBAuyVAh5cA7YYA7YYAXR4SoN0QoNGHBGjMggTofGP2BESMJUAjJgj5gwDtlgAdXgK0GwK0GwJ0eUiAdkOARh8SoDELEqDzjdkTEDGWAI2YIOQPArRbAnR4CdBuCNBuCNDlIQHaDQEafUiAxixIgM43Zk9AxFgCNGKCkD8I0G4J0OElQLshQLshQJeHBGg3BGj0IQEasyABOt+YPQERYwnQiAlC/iBAuyVAh5cA7YYA7YYAXR4SoN0QoNGHBGjMggTofGP2BESMJUAjJgj5gwDtlgAdXgK0GwK0GwJ0eUiAdkOARh8SoDELEqDzjdkTEDGWAI2YIJSe6dOny+DBg+Xbb7+VTz75xLsa9K644gpp0KCBdULly4022kh22mmnKHIfdNBB3t1tt91kyy23jE4AzW37UP/9J598chTqzdfPl3qhftlll0UX6+b++bBhw4by5z//WVZddVVr/7JgKQJ0nz59omP817/+1Xr9fKgh4JZbbpGXX37ZOv4+/PDDD6MLuIsvvtjati81Wm2xxRbyhz/8wTpGPkw7QGuk/+CDD+Smm26y9s2XTZs2lQceeEDee+896xj5UG9W3X777XLaaadZ2/bh/vvvLzvssEMUWc3j48s999xTrrvuOnnnnXes/fPhc889J2eddZb83//9n7VtH6611lrRTYxWrVpZ2/Zl69atoxufRxxxhHWMfHj66afLnXfeKa+99pq1bR9qQNfX59xzz7W2jaXzkksuif6uffTRR9YxyoJvv/223HvvvdH72dw3HzZu3Di6UbXxxhvLCiusYL3XfbjOOuvINttsI40aNbK270P9vXqDe+2117a2nRXTDtD6e9u1a5fa+ZF+Trds2TK1iSpZx+wJiBhLgEZMEErP8OHD5a233opmc2mg9K3OsNLgkNasT1WjlW5HTy6feOIJ715//fVy6KGHygYbbGBt24c6A00DvQZE8/XzZbNmzeSee+6x9s2X1157bRS311tvPWv/smApArReoHzxxRfRDFnz9fOhzuLSmat6LMzj78PLL788ird6sW5u25dXXnlldKGoAc48Rj5MO0DPmzdPBgwYEMUxc998ed9990U3GnQ2vXmMfHj11VdHfw8efPBBa9s+1Hh+5pln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otj7779vvX5ZUW8k/fOf/5TddtvN2j8fahjWC3X9rEiDUgToNdZYQzbZZBPZeuutrf3z4V/+8pfoBkBa0W2llVaSddZZJ4pj5rZ9ecEFF8ibb74ps2bNMg+RF/r37x9FsSOOOMLatg/1RpuGw9dee816j/hSY9W0adOiiJgGs2fPlgkTJsiQIUOsbWfF//znP9FY0htX5jHKgvq3Rv9uvvXWW9a++VA/R/XzVD/vshqgR40aFZ0/6kQG8/XLiieeeKI88MAD8u2331rHKAvqNaBeC6Z1wzDrmD0BEWMJ0IgJgo2eeLzzzjvSqlUradmypXfvuusuue2226IooyE6i7788svy/fffy/z56YyhtAP0WmutFV1Mn3XWWda++fLWW2+Nwp55/H2pMzL33XffKJqY++dD/b177bWXnH322da++VJnfd55553WvmXFa6+9Noo9Gq3M18+HpQjQgwcPlrfffjvaH/P4+FA/5/TzTj/3zNcvK+oY1bFq7psvH3744ehmhl7spkEpAnSdOnXk6KOPlmuuucbaPx/qzar99ttPNtxwQ2vbPvzjH/8ou+yyi5x++unWtn3Ztm1b6dmzZ2rfHBozZkw0W1/DmLltH+p7Oe3PbI3bP/74o1RWVpq75wV9jfRbH0899ZS17ayY9udR2pbib4J+u0pvDOu30cz3ug/TDtCTJ0+WL7/8Mhqn5uuXFZs3by6333673H333dbxyYL33XdfdG6k14RgY/YERIwlQCMmCDaffvppdOKkX33bddddvauzAvSE7IMPPoi+lp5FdfaQniCnNUsp7QCtM/X0K6w6+9bcN18+88wzcuGFF0rDhg2tMeBDnWGiyyakNSNw8803l3/84x9RNDH3zYe6FI1eXGlYMvctK+oMNH2dVl99dev182EpArTO7hk5cqT88MMP1jHyoc581tBw/PHHW69fVjzzzDOjSKxBwNw/H+rMVQ1jv/76q3l4vFCKAK1BRqPY559/bu2fD/Ur9fp5qjOszW37cO21146Wlvr3v/9tbduXumTPpEmTUluWScePjiMdT+a2fajH9sEHH4zinvke8aXOsO7YsaPMmTPH3D0v9OvXL5od26RJE2vbWbFp06bR3079G2oeoyyoS7ho3NPZvea++XKrrbaSdddd13qf+zLtAK03qcaNGxfFT/P1y4r6LT2d6X7QQQdZxycL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AO2WAB0WsycgYiwBGjFBsNGLrB9//FG6dOkiH374oXf15F5PLmfNmmVu2guTJk2St956S6699lo57LDDUvHSSy+VRx99VD744ANr/7LgK6+8IpdcconUrVvXOqn1oV4AHXDAAdK8eXPp3LmztX0fPvLII9GNgI022sjavg/TDtAacioqKqRPnz7Wvvmye/fuMnz4cJkzZ465eS/ozZ727dvLZZddZr1HsqIe47ffflumTJli7p4X9GaPvt8uvvhia9s+1Bim7+MNN9zQGsOmxxx7rBUOe/fpLfPn87cQao6OFx035li6/oYbrPFZSvU9pu81fc+lQSkCdKNGjaKo1LFjR+vz3Idpn3/p35oRI0ZEf3vMbfvy+++/j/52phVXCdBu9O+l3lz94osvrOPjQw3DeuP2rLPOst7nPjziiCOiOKw3n81t+/LBBx+UE044QTbYYAPrGPmQAB0WsycgYiwBGjFBsNGT++eff14uv/xyueiii7x7zz33ROFWL+bSQC+sNKC/88478vTTT6eizvps1qxZdMFr7l8WPPPMM2WfffaRTTfd1Dqp9WWdOnXk0EMPlQsvvNDavg91plj9+vVlzTXXtLbtw7QDtEYcvXDQ8WTumy8feOAB+eSTT2Tq1Knm5r0wbdo06d27t7z55pvWeyQr6udE3759U5txqK99r169ogtqc9s+1G97nH322dF7wRzDhe6w4w7yyeefFgXD73p2lzlz07k5AflGx42On8Lx9OU3X0nbF9taY7RU6ntM32tp3UwqRYDWvzv6rYymTZtan+c+TPv8Sz/v9G+O/u0xt+3Lp556Srp16yaVlZXm5r1AgHaj33569dVXo/Ng8/j48IorrpBbb71VHnroIet97kMdQ/fee28UcM1t+7JJkyZSr149WX311a1j5EMCdFjMnoCIsQRoxATBplOnTtHs2C222CK6c+/bQw45JJq9OmTIEHPTXtAT7tmzZ0dxbOLEianYrl27KPpowDX3Lwuut956ssYaa8jKK69sndT68g9/+IOstdZa1rZ9uc4668hqq62W2tqjaQfoefPmyRtvvBGNI3PffHnsscfKM888E80WSwON6BpuNTqY75GsqJ8T+nmR1oV62q+RzjbUWVyHH364NYar1PfK6/97oygWftutq8ycOdP85wLUGB0/Oo4Kx1W3Ht9Fy9mY47QU6ntM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fvQ8meeuVBKg/RCg/RCg3RCgy0MCdLExewIiphKgER1C6Zk6daoMHTpUOnfuLO+8805wNVidfvrpsuWWW1oDwlA2bdpUttlmGznggAOSSXVod9ttN9lggw0ym4RqhFh//fVll112sbYdSg2fOvh+++23rXMUwscff1zatGkjRx99tLXtEO69997J81EbNmxoHb8Q1q1bN7lJonHS3LdQ3nrrrXLIIYfIqquuam0/hGuuuaYcddRRcvfdd1vbDuG///1vufjiizOdhGpU3WKLLWTfffe1roEQtm7dWp555hkZPny4+VYYBF3x+uOPPyY3AczjF8obbrghea+rV6+edfxMm2/WXB557FErgC6yd78+Mnbc2CSSwNKjx1GPpx5X81gvUs+HnhfzXJVafT1kGaC//fbb5LV26qmnWq/DEOoNwwcffFAGDBhgbjoYPXr0kDvuuEMOP/xwa/shPPHEE+X666+XF154wXqd50W9yaB/O/VvqHmN5cHVVltNDj300CSwmvsWSh176Rjs4IMPtq6BEOrfZf37PHHiRPMSDkIRArTOQfTG7bPPPmudnzz47rvvJjf1xo0bZ54eEAI0oksCNKJDKD06sNTVGZdeemkyGQqtrvrYaaedkkhsDghDud122yUDS42gOqEO7VVXXZUEH52omNsOYaNGjaRly5bJRM7cdihvu+02ueSSS6zzE0qdXOlE+pFHHrG2HUKd/Gjc1lW+5vELoa5M0psAGrrNfQvlX/7ylySir7TSStb2Q6hBUidZOsk1tx1CXQm4xx57yDrrrGNtO5TNmzdPtqOfmjCvgRC+8cYb0q9fP/npp5/Mt8IgzJ07V7p165bcBDCPXyj1vUJXci+//PLW8fstW+7fUt74z7+tGLrIPp/3TVbsLvhlgblLsATocdPjp8fRPLaL1OOv58E8N7HUcKWviawCtL7G9LWmrznzdRjCl156KQnE+imrrPjhhx/kk08+SW6km9sPob7PXX311XLGGWdYr/O8qH8z9QZ9Vqt7s1b/Hm+++eZy4IEHWvsWSh176RjMPP+h7NChgwwZMkRmzpxpXsJBKEKAbtKkSTIX0TmJeX7yoN48f/TRR5P3VLAxewIiphKgER1C6dGPousEaOutt5ZVVlkluCuvvHISSurUqWMNCEP517/+VZ588slkwquT6dDq6vDzzjsvs/DWrFmz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MUMmTZpkbRurnDJlShJ7srpRkjULFy6UOXPmyNSpU619y4t6k0cD9G677Wa9l+TFBg0ayAorrJDcfDPfS0JYt25dWXnllaVRo0bWtvNiqY7Rqquuam07hI0bN5ZVVlkliVfmtkP5l7/8RR588EEZOXKk9ToJYadOneTcc8+V9ddf39p2CJdZZhlZccUVpWHDhtbxy4v6b19ppZWkTp061v6FUMeNp556ajKONM9PKPM+tujXr59cc801st1221nHL4T6Gj788MPlySeftLadF3/66SeZOXOmzJ8/3zw9UALMnoCIqQRoRIcAtaF///7JwPWcc85JIiXa6iqrm2++OZlMZ6GuTv7zn/+cBBlzchFCjQ377ruvXHLJJda2Q/jwww/LBx98IN9++615eQVj9OjR0q1bN3n66aet7WOVb775pgwaNCizFYdZoyFg6NChyQ0Zc9/y4l133ZXEBv1Eg/k+khcPPvhg2WyzzWT55Ze33ktCqO9HO+ywgxxzzDHWtvPiQQcdJM2bN88s4K622mqy0047JZ9OMrcdwpNOOkn23HPPzG56qvopgBNOOEHuuece63USwssuu0xatmyZhFZz2yHUcLvVVlvJYYcdZh2/vHjkkUfKNttsI/Xq1bP2L4QauPfee2+56KKLrPMTwoceekjee+89GT58eHKDMgvGjh2brOB+5plnrO2H8Prrr5cjjjgisxslenNh++23l9atW1vbzovPP/+8dO/eXcaPH2+eHigBZk9AxFQCNKJDgNqgqw8GDx4snTt3lo8++ghr8IEHHpCzzjpL9tlnnyQUh1ZXxugq5ayCj67UW2eddZLtmNsO4X777Sc33HCDfPLJJ+blFYwBAwbI/fffnzwOxdw+VnnhhRcmEVpXQucRXf2ssaFt27bWvuXFv/3tb9KuXTt56aWXrPeRvHjLLbfIgQcemKyQNd9LQqghRsPkE088YW07L950002y//77Jytkzf0L4YYbbignn3xycsPN3HYI33jjDTnvvPOkRYsW1rZDqRF98803l7322st6nYRQo9u6666b2TnQ1ee6svTuu++2jl9e1Lin70lNmza19i+EOmbRsUtWYwtVP1Gi49OsArSOf/U4HX300da2Q7jrrrsmj4mpX7++dfxCqJ/CaNKkiWy55ZbWtvOifsqwffv28tVXX5mnB0qA2RMQMZUAjegQAMKjk55XXnklmcTpR3LNwT+W5hmEXbt2TT5urR/5NbePVepKNL1ZktdVRPoR3KeeeipZgWvuW17U4Hb11VcnN0zyij6qp1WrVsnqRnP/QqirSnWVuIafvKKPHDjxxBMzi/S6alVv6mUVZPTGs0Y3jT/mtrHKNddcM/lkWJcuXczDlxt69uyZfPJJvwfC3L88WITv4EC/esNNP9HQu3dv8/RACTB7AiKmEqARHQJAeAjQfgnQ5SEBOr4EaL8EaL8E6PgSoONLgK4MCdBxMXsCIqYSoBEdAkB4CNB+CdDlIQE6vgRovwRovwTo+BKg40uArgwJ0HExewIiphKgER0CQHgI0H4J0OUhATq+BGi/BGi/BOj4EqDjS4CuDAnQcTF7AiKmEqARHQJAeAjQfgnQ5SEBOr4EaL8EaL8E6PgSoONLgK4MCdBxMXsCIqYSoBEdAkB4CNB+CdDlIQE6vgRovwRovwTo+BKg40uArgwJ0HExewIiphKgER0CQHgI0H4J0OUhATq+BGi/BGi/BOj4EqDjS4CuDAnQcTF7AiKmEqARHQJAeAjQfgnQ5SEBOr4EaL8EaL8E6PgSoONLgK4MCdBxMXsCIqYSoBEdAkB4CNB+CdDlIQE6vgRovwRovwTo+BKg40uArgwJ0HExewIiphKgER0CQHgI0H4J0OUhATq+BGi/BGi/BOj4EqDjS4CuDAnQcTF7AiKmEqARHQJAeAjQfgnQ5SEBOr4EaL8EaL8E6PgSoONLgK4MCdBxMXsCIqYSoBEdAkB4CNB+CdDlIQE6vgRovwRovwTo+BKg40uArgwJ0HExewIiphKgER0CQHgI0H4J0OUhATq+BGi/BGi/BOj4EqDjS4CuDAnQcTF7AiKmEqARHQJAeAjQfgnQ5SEBOr4EaL8EaL8E6PgSoONLgK4MCdBxMXsCIqYSoBEdAkB4CNB+CdDlIQE6vgRovwRovwTo+BKg40uArgwJ0HExewIiphKgER0CQHgI0H4J0OUhATq+BGi/BGi/BOj4EqDjS4CuDAnQcTF7AiKmEqARHQJAeAjQfgnQ5SEBOr4EaL8EaL8E6PgSoONLgK4MCdBxMXsCIqYSoBEdAkB4CNB+CdDlIQE6vgRovwRovwTo+BKg40uArgwJ0HExewIiphKgER0CQHgI0H4J0OUhATq+BGi/BGi/BOj4EqDjS4CuDAnQcTF7AiKmEqARHQJAeAjQfgnQ5SEBOr4EaL8EaL8E6PgSoONLgK4MCdBxMXsCIqYSoBEdAkB4CNB+CdDlIQE6vgRovwRovwTo+BKg40uArgwJ0HExewIiphKgER0CQHgI0H4J0OUhATq+BGi/BGi/BOj4EqDjS4CuDAnQcTF7AiKmEqARHYLN2LFjpV+/fvL222/Lm2++GVydmHz99ddJOMkrY8aMkb59+8pbb71l7R9Wec8998h5550nhx56aC7db7/9ZIsttpBGjRpZA/8Q1qlTR/bYY49kAmEeu1A++uij0qZNmyR0m/uXBzWqbrvttknUMI9fKLMO0DNmzJBhw4ZJ586drfMTQo0MepNh++23t/YtlKuvvrr88Y9/lAMOOMA6RyE8++yz5YUXXpDvvvvOPHxBmDdvnvzwww/So0cP6/iFsl27dsl7xsorr2wdvxCuu+66yev4/vvvt7Ydwo4dO8oXX3yR2etAyXuAnjZtWnKcrrzySusaDuWOO+6YRD39+2DuXx4sRYDOevz12GOPyRVXXCHHHXecdX7yoo69dAxm7lso27dvL23btpVjjz3W2nYIsx5/6cKIZs2ayQ477GBtO5S77767bLDBBrLCCitY2w8hATouZk9AxFQCNKJDsNH4/Mgjj8jJJ58s//znP4OrK900bo8bN87cdG7o06ePPPTQQ8lk2tw/rPLiiy+W22+/XZ599tlcetdddyXBZ6ONNrIG/iHUCZCusNprr72sYxdKDZMaZHRCbe5fHnziiSfk9NNPl+222846fqHMOkDrDT0NJRqtzPMTQr1Gd911V/nDH/5g7Vsot9xySznppJOS9zzzHIVQj4/Gz8mTJ5uHLwizZs1Kgpi+H5nHL5T77LNP8l6x3HLLWccvhLqyWm8CHHLIIda2Q6ivM329DRo0yDx8wch7gJ4zZ05yM+mDDz6wruFQnn/++bLbbrtldh1lbSkCdNbjL70hdt111yWR1Tw/eVHf63QMZu5bKPUY6Wstq2OU9fhLb/DsvPPOyRjJ3HYo9SZGy5YtM/tUDAE6LmZPQMRUAjSiQ7DROKzxedVVV5UVV1wxuDq5uuOOO2T48OHmpnODrgA54YQTpEGDBtb+4Yqy0koryTHHHCMvvvhistJ99uzZuXPo0KFy8803JyvSzIF/KHUSpKHBPH6h1I+K6yqoESNGWPuXBzVIPvnkk0l0M49dKLMO0Pppj9tuuy2Z7JrnJ5R6DWW5YlKvI115O2rUKOschXDu3LnJKuWsPi6uK1effvrpZFWaeexCqeegbt26ycffzeMXQr1hpY/tWX755a1th1Af03PWWWfJhx9+aB6+YOQ9QOujpebPn59cr+Y1HEp9dJWGNz0n5v7lwVIE6KzHX7pyVd+z9Toyz08e1BtuL730UnId6ScyzP0Lod44v/POO+Wbb76xth/CrMdf+l6qj4jTMaq57VDqjaozzzwzs09wEaDjYvYEREwlQCM6BBsd3OsKBx1kmgOeEOpHxXVgqQPXvPL6668nHz3M6yqlrC3FMwizRh8HoKuINBya+5cX99xzT7n33ntl9OjR5u7lAl1x+Nxzz8lhhx1m7Vsosw7QumLyxhtvlBYtWljbzov6cWj9VMykSZPM3csFU6dOlccffzx5hIi5b1il3nA+9dRT5f333zcPXzDyHqBLQdbjr6wtRYDOevy10047JQE6q0cCZU0pvoNDP3Wjq5RHjhxpbj4IWY+/ivAdHATouJg9ARFTCdCIDsEm6wkQAbr4EqDLQwK0XwK0XwJ08SVAlwdZj7+ylgAdHwK0XwI0LC1mT0DEVAI0okOwyXoCRIAuvgTo8pAA7ZcA7ZcAXXwJ0OVB1uOvrCVAx4cA7ZcADUuL2RMQMZUAjegQbLKeABGgiy8BujwkQPslQPslQBdfAnR5kPX4K2sJ0PEhQPslQMPSYvYEREwlQCM6BJusJ0AE6OJLgC4PCdB+CdB+CdDFlwBdHmQ9/spaAnR8CNB+CdCwtJg9ARFTCdCIDsEm6wkQAbr4EqDLQwK0XwK0XwJ08SVAlwdZj7+ylgAdHwK0XwI0LC1mT0DEVAI0okOwyXoCRIAuvgTo8pAA7ZcA7ZcAXXwJ0OVB1uOvrCVAx4cA7ZcADUuL2RMQMZUAjegQbLKeABGgiy8BujwkQPslQPslQBdfAnR5kPX4K2sJ0PEhQPslQMPSYvYEREwlQCM6BJusJ0AE6OJLgC4PCdB+CdB+CdDFlwBdHmQ9/spaAnR8CNB+CdCwtJg9ARFTCdCIDsEm6wkQAbr4EqDLQwK0XwK0XwJ08SVAlwdZj7+ylgAdHwK0XwI0LC1mT0DEVAI0okOwyXoCRIAuvgTo8pAA7ZcA7ZcAXXwJ0OVB1uOvrCVAx4cA7ZcADUuL2RMQMZUAjegQbLKeABGgiy8BujwkQPslQPslQBdfAnR5kPX4K2sJ0PEhQPslQMPSYvYEREwlQCM6BJusJ0AE6OJLgC4PCdB+CdB+CdDFlwBdHmQ9/spaAnR8CNB+CdCwtJg9ARFTCdCIDsEm6wkQAbr4EqDLQwK0XwK0XwJ08SVAlwdZj7+ylgAdHwK0XwI0LC1mT0DEVAI0okOwyXoCRIAuvgTo8pAA7ZcA7ZcAXXwJ0OVB1uOvrCVAx4cA7ZcADUuL2RMQMZUAjegQbLKeABGgiy8BujwkQPslQPslQBdfAnR5kPX4K2sJ0PEhQPslQMPSYvYEREwlQCM6BJusJ0AE6OJLgC4PCdB+CdB+CdDFlwBdHmQ9/spaAnR8CNB+CdCwtJg9ARFTCdCIDsEm6wkQAbr4EqDLQwK0XwK0XwJ08SVAlwdZj7+ylgAdHwK0XwI0LC1mT0DEVAI0okOwyXoCRIAuvgTo8pAA7ZcA7ZcAXXwJ0OVB1uOvrCVAx4cA7ZcADUuL2RMQMZUAjegQbLKeABGgiy8BujwkQPslQPslQBdfAnR5kPX4K2sJ0PEhQPslQMPSYvYEREwlQCM6BJusJ0AE6OJLgC4PCdB+CdB+CdDFlwBdHmQ9/spaAnR8CNB+CdCwtJg9ARFTCdCIDsEm6wlQ1gF63rx5SUzSSWifPn0yUScn+++/fzKINfcPqwL0Pvvsk5xnHRybxy8PvvXWW3LeeefJlltuae1fXsx7gP7555+lY8eOctFFFyXvG1moIeOOO+6Qjz/+2LoGQqix5Mwzz5RNN93UOj95UYNMmzZt5MMPP7T2L4SDBw9OrtHZs2ebl0AQShGgGzRoIOutt15yo8G8xvKgvlecf/758uyzz1rnJ5T33HOPHHzwwbLSSitZxy+ERQjQGm6vvvpq2W233axzlAf17/7FF18sL774onX+Q/nYY4/JGWeckbwvmdsP4QknnJC8DsaMGWOeniDo37Vx48bJ0KFDrX0L5aOPPiqnnXaa7LDDDtb+hVDj9i233CIffPCBte0QZj3+qlu3bjKGv/XWW61th7J9+/Zy9NFHy+qrr25tP4R/+MMf5Pjjj5dnnnnG2nYI+/XrJyNGjJApU6aYlzAIARrRJQEa0SHY5D1AT58+XT777LNkAH755Zdn4uGHHy6bb7651KlTx9o/rLJ58+Zy6KGHJis0zOOXB3WCq6tjs1q9UgrzHqDnz5+fxEmNuPqekYXXX399Enw0sJrXQAh1Velee+0lTZo0sc5PXtSwqpN1DQLm/oVQV9JpyBg7dqx5CQShFAF6k002kSOPPFLatWtnXWN58KabbpJrr71WrrzySuv8hFI/FbPVVltltnK1CAH666+/lrfffju5KWaeozy46Dq64oorrPMfyquuukquu+46a9uh1KDXs2fP5H0jC/T3fvrpp/Lwww9b+xbKrI+Rvs70vS6rv5tZj790ZbiO4XUsb247lMcdd5xst912mX3io2HDhskKcb1hYm47hPq34OWXX5YhQ4aYlzAIARrRJQEa0SHY5D1AT5gwIVm9oisbN9poo0xs2rSp1KtXL1npa+4fVqmDbj1O5rHLi+uuu66sttpqssIKK1j7lhfzHqD1o8R6Q0lXoun7RRZq3NaVn/q+ZF4DIdTrqHHjxrL88stb5ycv6opVXcW1/vrrW/sXwr/85S/J6tiswmEpArS+1jS+6aox8xrLg59//rncd999ycpG8/yEUh/PoH83s3osQBEC9MyZM5PVscOHD7fOUR7s1auX3HnnnUnYM89/KDW46SpoXUFsbj+EP/74Y7LqU2+AZoHeaHvqqacye61tvPHGctJJJ8mTTz6Z3NAw9y+EukL5kksukV122cXafgizHn/p2D3rMWqzZs2ST8boamtz+yHUT2A2atQoWQltbjuEW2yxRfLps06dOpmXMAgBGtElARrRIdjkPUBrrNKJtK46NLeNWEnmPUCXAn30xllnnSVrrLGGdfywNOpKNF2FPmDAAPP0BKEUAVojukaxvH5c+aeffko+NdSyZUtr3/JiEQJ03inF+OuII46QF154IXmURR7RwH333Xcnj1kx9y2EpfgODv2U4QUXXCBrr722tX0shhr///GPf8i///1v8/SDEKARXRKgER2CDQEasRgSoP0QoONLgI4PARpCUIrxFwHaLQEaQ0iAdmP2BERMJUAjOgQbAjRiMSRA+yFAx5cAHR8CNISgFOMvArRbAjSGkADtxuwJiJhKgEZ0CDYEaMRiSID2Q4COLwE6PgRoCEEpxl8EaLcEaAwhAdqN2RMQMZUAjegQbAjQiMWQAO2HAB1fAnR8CNAQglKMvwjQbgnQGEICtBuzJyBiKgEa0SHYEKARiyEB2g8BOr4E6PgQoCEEpRh/EaDd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B7d9+/Zy5ZVXJufhiCOOCK6GmK233loaN25snZ8Q6gRo1113lYsuuigZ4Jv7F0Kd4Oo1lNVEPWsWLFgg3bp1k9tuu806P6E8/fTT5aabbpLnn3/eOn4hfOihh5JrNKuYUYoJkEal/v37yzvvvGPtXwgfeOABOe6442T99de39i8v/vGPf0yupaefftravzz48ssvJ6+zs88+23qNhPK6666TTp06ycyZM81LLAg//PBDsh/nnXeetW2s8pRTTpFrr71WnnrqKesaCKHeMNSxyxlnnGFtO5Tnnnuu3HHHHfLKK69Y2w+hvg4OP/xwadq0qfU6D2GjRo3kwAMPlOuvv97aNlap47uTTjpJmjdvbh2/EJZi/JW1ehNGbyadeuqp1mskhHrj/8ILL5R77rnH2nZefOaZZ5LXWevWra39C+HRRx+dHJ9evXqZf45ACNCILgnQiA7BhgDtR+OzhqXjjz9ejj322OCeeeaZycBSV+298MILwb3//vuTc7zJJptY5yeU66yzjuy+++7JKhNz/0KoqwE7duyY21WZujLp+++/l08++cQ6P6HU89y2bdtksmsevxAecsghyarDrG5klCJADxkyJAlWGljN/QvhwQcfnARcDTPm/uVFjVU77rhjMmk39y8P6vv0ZZddlkymzddIKDt37pysSPv555/NSywIurJaV0HrjRJz21jlww8/LFdffXUSZMxrIIQnnniitGnTRu677z5r26G86667kk8/6fueuf0Q7r///rL55ptLvXr1rNd5CHXcqOOKfffd19o2VnnYYYfJtttuK6uvvrp1/EKZ9fgra/Vmki7C0E8cmK+REOpNeV1hrRHa3HZebNWqlVx11VXJQmo6h9sAAIAASURBVABz/0KoNzy7d++erNgHG7MnIGIqARrRIdgQoP1oENOoscoqq8hyyy0XXF29oiuV9Bhp0AjtiBEjklVWu+yyi3V+QrnMMstI3bp1rX0L5aLHS+R5cKwRev78+db5CeXHH3+crKhbd911reMXQv0Ya506dZIVV+b5D2EpArSupNdj1KxZM2v/Qpj1MSqFWb+Ws3a11VZLJusab83XSCj1dZzlx90XLlyYfGpi3rx51raxSn28xBVXXJE8psS8BkK41lpryVlnnSUffPCBte1QvvHGG8nYokGDBtb2Q5j1+5H+Xv39uh1z21hl1udAzft7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0OVDgG4OCdBYBQnQfgnQzQEB2m/V97/Aje4JiJhKgEZ0CACQBQG6fAjQzSEBGqsgAdovAbo5IED7rfr+F7jRPQERUwnQiA4BALIgQJcPAbo5JEBjFSRA+yVANwcEaL9V3/8CN7onIGIqARrRIQBAFgTo8iFAN4cEaKyCBGi/BOjmgADtt+r7X+BG9wRETCVAIzoEAMiCAF0+BOjmkACNVZAA7ZcA3RwQoP1Wff8L3OiegIipBGhEhwAAWRCgy4cA3RwSoLEKEqD9EqCbAwK036rvf4Eb3RMQMZUAjegQbD788EPz97//3cyZM8fMnj0bsVM6f/58895775lPPsnnfSzbuOSSS0zXrl3NOuusE9yvfe1r5otf/KL5j//4D+vAJZRyIH3yySebKVOmWI9fFZw5c2YSDiU26MevKq611lpm9dVXN6ussor1/OByYwjQ//7v/266dOlivvrVr1qvgSoof7f8/XI/9H0LZctjJJ99evtV8Ac/+IEZMGCAmThxon6JVQYCtN+8A7Tss8i+y4IFC6zvvFAuWrQo2ZfPCwK036rvf8kxoBwL5vk6qjK6JyBiKgEa0SHYSPQZO3asueCCC8y5556L2CkdNWqUefrpp827776rX8JBkB3vBx54INlW7969gysH6RLE8goB4je/+U2zyy67mMGDB1uPXxU855xzzJAhQ5Loox+/qrj//vub//u//0tGT+rnB5cbQ4Beb731zDbbbGMOP/xw6zVQBbt372623nrr5H7o+xbK9ddf3/z6179ORhHr7VfBgQMHmtGjR5uXX35Zv8QqAwHab94BWuKz7LvIPoz+zgvheeedZ+68887kdfqPf/xDbz4IBGi/Vd//kmNAORacMWOGfvjAEKARXRKgER2CzYQJE5Kz9ltttZX52c9+htgpPeyww8zIkSOTkdB5sHTp0mQEiEQxGfEWWjlAP+mkk8yWW25pHbiE8gtf+IL5xje+YX784x9bj18V/MUvfmF69uxpLr30Uuvxq4o33nhj8lrdYIMNrOcHlxtDgJZoJWFPDtj1a6AK3n333aZfv35m0003te5bKDfbbLMk4t53333W9qvg448/npygl9dTVSFA+807QMvIZ4nPctJHf+eFcPPNNzcnnHBCMlXMp59+qjcfBAK036rvf/3yl79M4rkcE4KN7gmImEqARnQINrfffrvZd999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6CgEaKyKBOjyJUCXDwHaDwG6OSRA+yFA+yFAx43uCYiYSoBGdAg2BGjoKARorIoE6PIlQJcPAdoPAbo5JED7IUD7IUDHje4JiJhKgEZ0CDYEaOgoBGisigTo8iVAlw8B2g8BujkkQPshQPshQMeN7gmImEqARnQINgRo6CgEaKyKBOjyJUCXDwHaDwG6OSRA+yFA+yFAx43uCYiYSoBGdAg2BGjoKARorIoE6PIlQJcPAdoPAbo5JED7IUD7IUDHje4JiJhKgEZ0CDYEaOgoBGisigTo8iVAlw8B2g8BujkkQPshQPshQMeN7gmImEqARnQINlUP0MuWLTNvvPGGmTZtWrKTjLZTpkxJDq7effdd/fAFgQCNIZTXzpprrmm+853vJM93HspnnTzP48aNs94nIRwzZoz505/+ZLp162ZtG5e71157mcsuu8zMmDFDvw2D8MknnyQnGF544QXr+Qnl888/b958803z8ccf680H4YMPPkg+66ZOnWptO4T33HOP6d+/v9l0002t92FV/Pa3v20OPfRQc+ONN1r3L4R33XWX6du3r9lkk02sbYeSAF2+ckLsiCOOMLfccov1GsDlysmqPfbYw3zlK1+xHr8quPLKK5tf/epXyclhfd9COWLECNOvXz+z7bbbWt95VXDLLbc0gwcPTkI62OiegIipBGhEh2BT9QD9zjvvmAkTJiSjGmWEBtqedNJJyaiGV155RT98QSBAYwjlIFFiz4EHHpg813kon0Unnnii6dOnj/U+CeGxxx5rBg0aZE4//XRr27jca665xjz22GNm4cKF+m0YhCVLlpjJkycn0UQ/P6GUdU+aNCm3cCgnVSUSn3baada2Q3j44YebbbbZxqy33nrW+7Aqyskq+V7Yf//9rfsXwsMOOywZ/ZznqE8CdPmutdZa5uc//3nyvaNfA7hcic8bb7yxWW211azHrwrKfumGG25odthhB+u+hVLeZyeffHKyH6m/86rg+eefn3zn5HViuOronoCIqQRoRIdgU/UALSPdrrrqKrPbbrslB4poK6PcJELLwWgeEKAxhKuuuqrZeeedk9Gx8jznoXze9erVy3z/+9+33ichlNfpwIEDzfjx461t43LnzZtn3n77bfPRRx/pt2EQ5EqPkSNHJt9r+vkJpVyqLCNv5QRoHkyfPt2cffbZZuutt7a2HcKvfe1r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efPmmRtvvNF0797devxCuNVWWyXR7aGHHtKbDsbbb79tZsyYYSZPnpwcDFXNRx55xEyfPj15T+fF008/bc4991zTtWtX6zkK4Y9//GPz9a9/3fznf/6n9RoOYREBevHixebll182jz32mPUchVAigLwXdtttN+vxC+GvfvUrc/TRR5vLL7/c2nZVnDJlipk1a5b54IMP9NMThCIC9GabbWaOP/54M3bsWOv+VUF5/ct+y1tvvaUfvmDIZ96QIUOS16x+HYfwkEMOMdddd52ZM2eO3nQQitj/ku/kCy64IDl5qJ+jEE6dOtXMnj3bLFmyRN+9SlDE/tf3vvc907NnTzN69Gjr8QvhmDFjzNChQ83ee+9tPf8hLGL/669//asZNmyY2Xnnna3th1D2LdZff32z2mqrWc9PCAnQbnRPQMRUAjSiQ7CpeoB+//33kx0/iRqy84e2V1xxRbKTL6OV8qCIA6C8A/Q777xjnnrqKTNq1Cjr8Qvh8OHDkxCT1/tAkPgs74MTTjghGYlTNY855hhz7bXX5naiRJDQ8OCDD5rLLrvMeo5CKI/9DjvskFx1oF/DISwiQL/44otm5MiR5rjjjrOeoxD26dPHDBo0KBnxph+/EMoJBol6ef39RSjfmffcc09yYiwPigjQciJmm222SQKivn9VcMCAAcn3mpyMyYuZM2eacePGmQsvvNB6HYdQ3sdyMkNOTuZBEftfEibludDPTyjPPvtsc//99yfviSpSxP7X2muvbX7+85+bAw44wHr8Qijf/X/84x/NGWecYT3/ISxi/0tO8sh+tuxv6+2HUB4fOXG+7rrrWs9PCAnQbnRPQMRUAjSiQ7CpeoD+5JNPkoOghQsXmrlz52KGb775ZnIAmtclrEUcAOUdoOV1JJeK5/U6kvgvI0vzHGUlI58lgP7oRz9KLgOtmjK6R0ZZSXjLi6VLlybvBQl7+jkKoYxolPApI8b0aziERQRoOYju16+f+e53v2s9RyH82c9+lhxMy3b04xfCl156yVx88cWmW7du1raroozYkzAm9yUPigjQEjTWWGONJF7p+1cFN9poo2QEd15TVwnyfSAjrOX7Qb+OQ7hgwYLke+3jjz/Wmw5CEftfMoJbwud6661nPUchlKliLrrootz2LfKmiP2vz372s+YLX/iC+a//+i/r8QvhJptsknznyMkY/fyH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AI0Se4Bu4cWXXjK7/fMAUa9TlAMJF80WoFuQv+HwI3pY6xLl8s/OCgEaAKCajL9/vPX5re11zNHmqquvtv57ngFaYl/WfO+/32uv5IcsOxqKn3v+edO337HW+mV/qaPrLhoCNEC10D0BEVMJ0IgOARqlWQK08ML0FzLX6/t7mjVACzLiS1+23qKM+OyMEKABAKpJrQC99377mquvvcbM/df8yK/Pnm0tk1eAlmlHJC7q7Z03bJj5MPDVBjLfddff7dRmO535hG8WBGiAaqF7AiKmEqARHQI0SjMFaOGSP19qrffXv93WGSibOUALMvoq6wegZER5UT+01ggEaACAatI6QHfd+Xfm7HPOSaaF0hQVoOX7T6adar0d+ZHRBx56UC8ajL/N+JvZfc/ft9mmTNVRFQjQANVC9wRETCVAIzoEaJRmC9ALFixIfmler1vmTa1FswdoQR63HdWoLFEONDsbjQboBQsXmjfffDP5v0Uyf/78ZLtivXOGhkbiSsvfUNRl3vJctGxTRtgXwSeffLJim2+/87b+506NjLBM/u63i/+75UdaWx63In58TrNo0aLl2//ne6UqtDxfRf7NS5cuTV/fJbxOQiJz2g44YWASeF1zThcVoM87f1ibbWy/447mr888oxcLzquzZrX5kTmJ3q+//rperMPIj5W2vHbkczIEnTVAt76vn376qf7nhmg0QLf+rq1nPw6gSHRPQMRUAjSiQ4BGabYALRx0yMHWuuXS3loQoJcz4vrrrPXuuc/edR20Tntumul3XP/kMmqZv9MV/DuKL0DLa/u6G643R/bqlRzUt15uh526mqP79jE333JL8OD28ssvJyPwDz28u/X6lr/54MMONcMuOD9ztF+9yFykZ5x1Vht1WJdwc9nll5tDDjvMOhmzc7ddzaDBJ5l7xo4NFqRnvvKKufLqq5L5U/XjLXbbY3dzwqA/mr/ceWddr+96kQAo88RKFNCvCQk7ErnkcvfWkevdd9+1Hj95P7mYOnWqdRu5Ly5ee+016zZj/nLHin+X1568RvVnlVx1kBXtL73sz9b62hNAJc7LSEv5jG4dv1qUEal9ju2bBCY5KdUol195RZu/8ZoR17b598WLF5sbR45MrrjYvuuObbYtrx15z8q2XSeUXJx5dtvHSPRNoSA/LNd6+XPOO1cvkny+yUjdff+wv/WYyWfe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7/zze3fceWfv6aef7t+wMLOb//nPf/b//8efeKJ340039k7+8SmNqy5OO+P08uFERJYtZZ8ALFBAQwsREZH1msxEPPJ7RzUumZ9EtkkR95aZz51FAb0yM0kBffY555Sbrbv85z//6Z3y4x/3PvaJjRo/n6XIDVBFRDZUyj4BWKCAhhYiIiLrPc8++2zvhyccv+jGhKNs9MmNe4cdcUR/hqJ0GwX0ykxbAf2Rj3+sv2yNdYoX8uqrr/aOP/HE3qc3+Wzj5zWNnPTa/8ADeg8//HD5ECIiy5ayTwAWKKChhYiIiPxv3n///X6xfPW11/RnLn7vmGP6Uh5dctmlvd/ed99cbiQmk+WOO+/orzM87I033iiHyTInaxx/c999el/44tb9UnTLrbfq7bzrLv2lIZ57/vlyuPxf3n777d5tv7qtvzZ2ZjGXBXPNZp/fvPetgw/uXXHVlf0iW0RkQ6fsE4AFCmhoISIiIiIiy5uczMo62b/73e8WnWS5/Y47en/84x97b775ZrmJiMgGT9knAAsU0NBCRERERERERGRcyj4BWKCAhhYiIiIiIiIiIuNS9gnAAgU0tBARERERERERGZeyTwAWKKChhYiIiIiIiIjIuJR9ArBAAQ0tRERERERERETGpewTgAUKaGghIiIiIiIiIjIuZZ8ALFBAQwsRERERERERkXEp+wRggQIaWoiIiIiIiIiIjEvZJwALFNDQQkRERERERERkXMo+AViggIYWIiIiIiIiIiLjUvYJwAIFNLQQERERERERERmXsk8AFiigoYWIiIiIiIiIyLiUfQKwQAENLf773/IjRURERERERERkIekOyj4BWKCAhhbvK6BFREREREREpCXpDso+AViggIYW72ugRURERERERKQl6Q7KPgFYoICGFu8poEVERERERESkJekOyj4BWKCAhhbvvaeAFhEREREREZHRSXdQ9gnAAgU0tHhXAS0iIiIiIiIiLUl3UPYJwAIFNLR4+10FtIiIiIiIiIiMTrqDsk8AFiigocW/336//FwREREREREREfkw6Q7KPgFYoICGFv/6jwJaREREREREREYn3UHZJwALFNDQ4s1/v1d+roiIiIiIiIiIfJh0B2WfACxQQMMY/7UMtIiIiIiIiIhUks6g7BGAxRTQMMZ772ugRURERERERKSZdAZljwAspoCGMd55VwEtIiIiIiIiIs2kMyh7BGAxBTSM8a+33YhQRERERERERJpJZ1D2CMBiCmgY481/uRGhiIiIiIiIiDSTzqDsEYDFFNAwAetAi4iIiIiIiMhwrP8Mk1FAwwTetg60iIiIiIiIiAwlXUHZHwBNCmiYwL/+Yx1oEREREREREVlIuoKyPwCaFNAwof+aBC0iIiIiIiIiHyQdQdkbAHUKaJjQO5bhEBEREREREZEPko6g7A2AOgU0TMgyHCIiIiIiIiKSWH4DJqeAhim8bxK0iIiIiIiIyLpOuoGyLwBGU0DDFN5+xyxoERERERERkfWcdANlXwCMpoCGKbz17/fKzx0RERERERERWUdJN1D2BcBoCmiY0rvvWYdDREREREREZD0mnUDZEwDtFNAwpbfcjFBERERERERkXSadQNkTAO0U0DCD98yCFhEREREREVlXSRdQ9gPAeApomMG/zIIWERERERERWVdJF1D2A8B4CmiY0XvvmwUtIiIiIiIish6SDqDsBYDJKKBhRmZBi4iIiIiIiKyPmP0Ms1NAwxLk7rciIiIiIiIisnaTY/+yDwAmp4CGJXjz3+/1VNAiIiIiIiIiazM55s+xf9kHAJNTQMMS/ecdS3GIiIiIiIiIrMXkmL/sAYDpKKBhDt53Q0IRERERERGRNZUc65fH/8D0FNAwB25IKCIiIiIiIrK24saDMB8KaJiTt981C1pERERERERkLSTH+OVxPzAbBTTM0XuW4hARERERERFZ1cmxfXm8D8xOAQ1zlDvj/lcHLSIiIiIiIrIqk2P6HNuXx/vA7BTQMGf/ftt60CIiIiIiIiKrMTmmL4/zgaVRQEMHrActIiIiIiIisrpi3WfohgIaOvLue0poERERERERkdWQHMOXx/XAfCigoUNuSigiIiIiIiKysuOmg9AtBTR06M0PvK+EFhEREREREVmRyTF7jt3L43lgfhTQ0LHcPVcHLSIiIiIiIrKykmP1HLOXx/HAfCmgYRkooUVERERERERWTpTPsHwU0LBM+iW0FlpERERERERkg6a/7IbyGZaNAhqWUdaVcmNCERERERERkQ2THJNb8xmWlwIaNoB331NCi4iIiIiIiCxncixeHp8D3VNAwwby9rtKaBEREREREZHlSI7By+NyYHkooGED+vfb7/f+q4cWERERERER6SQ55s6xd3k8DiwfBTRsYLnxgXWhRUREREREROab/nrPbjYIG5wCGlYIS3KIiIiIiIiIzCeW3ICVQwENK8i//vN+732zoUVERERERERmSo6pc2xdHm8DG44CGlag/7zzfk8NLSIiIiIiIjJZcgydY+ny+BrY8BTQsEJlnap331NDi4iIiIiIiLQlx87WeoaVSwEN49Fz2QAAO6tJREFUK1wuHXKTQhEREREREZHFybGy5TZg5VNAwyrRL6LNiBYREREREZF1nhwbK55h9VBAwyrz1gcfspbmEBERERERkfWWHAvnmLg8TgZWNgU0rFJZ3+rtd97vWZ1DRERERERE1mpyzJtjX2s8w+qlgIY1IJcevfPuf3v/VUaLiIiIiIjIKk+ObXOMa5kNWBsU0LDG5AP67Q8+qN24UERERERERFZLcgybY1mlM6w9CmhYw978wL/eXiikzZAWERERERGRDZ0cm35YOH9wzJpj1/J4Flg7FNCwzmTdrJxR/vf/FdO5iUM++COTpkNRLSIiIiIiItMmx5KD48rBcWaOOXPsmWPQHItayxnWHwU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJBTQAAAAAAJ1QQAMAAAAA0AkFNAAAAAAAnVBAAwAAAADQCQU0AAAAAACdUEADAAAAANAJ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AABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAhoAAAAAgIWABgAAAABgIaABAAAAAFgIaAAAAAAAFgIaAAAAAICFgAYAAAAAYCGgAQAAAABYCGgAAAAAABYCGgAAAACAhYAGAAAAAGAhoAEAAAAAWAhoAAAAAAAWAWtlgBzRCHN+AAAAAElFTkSuQmCC';

// ─── APP OBJECT ───────────────────────────────────────────────────────────────
window.App = {
  showScreen,
  requestNotifPermission,

  goUserBooking: function() {
    showScreen("screen-user-booking");
    setTimeout(() => {
      const lat = state.userLocation.lat || 17.9689;
      const lng = state.userLocation.lng || 79.5941;
      initLeafletBookingMap(lat, lng);
      if (state.userLocation.lat) updateLeafletPickupMarker(state.userLocation.lat, state.userLocation.lng);
    }, 300);
    if (state.userProfile.name) document.getElementById("user-name-input").value = state.userProfile.name;
    if (state.userProfile.phone) document.getElementById("user-phone-input").value = state.userProfile.phone;
    if ("Notification" in window && Notification.permission === "default") {
      document.getElementById("notif-banner").style.display = "flex";
    }
  },

  showAdminLogin: function() { showScreen("screen-admin-login"); },

  setPickupMode: function(mode) {
    state.pickupMode = mode;
    document.getElementById("pm-live").classList.toggle("active", mode==="live");
    document.getElementById("pm-manual").classList.toggle("active", mode==="manual");
    const input = document.getElementById("pickup-input");
    if (mode === "live") {
      input.readOnly = false;
      if (state.userLocation.address) input.value = state.userLocation.address;
    } else {
      input.readOnly = false;
      input.value = "";
      input.placeholder = "Type your pickup address";
      input.focus();
    }
  },

  pickupInputChanged: async function() {
    if (state.pickupMode !== "manual") return;
    const val = document.getElementById("pickup-input").value.trim();
    const sugEl = document.getElementById("pickup-suggestions");
    if(val.length<2){sugEl.style.display="none";return;}
    clearTimeout(_deb);_deb=setTimeout(async()=>{
      const results=await fetchPlaceSuggestions(val);
      if(!results.length){sugEl.style.display="none";return;}
      sugEl.style.display="block";
      sugEl.innerHTML=results.map(p=>`<div class="suggestion-item" onclick="App.selectPickupSugg('${encodeURIComponent(p.display_name)}','${p.lat}','${p.lon}')">📍 ${p.display_name.substring(0,60)}</div>`).join("");
    },250);
  },

  selectPickupSugg: function(name, lat, lng) {
    const decoded = decodeURIComponent(name);
    document.getElementById("pickup-input").value = decoded;
    document.getElementById("pickup-suggestions").style.display = "none";
    state.userLocation.lat = parseFloat(lat);
    state.userLocation.lng = parseFloat(lng);
    state.userLocation.address = decoded;
    if (bookingMap) { bookingMap.panTo([parseFloat(lat), parseFloat(lng)]); }
    updateLeafletPickupMarker(parseFloat(lat), parseFloat(lng));
    if (state.dropLocation?.lat) calcRouteAndFare();
  },

  recenterMap: function() {
    if (state.userLocation.lat) {
      reverseGeocode(state.userLocation.lat, state.userLocation.lng);
      if (bookingMap) bookingMap.panTo([state.userLocation.lat, state.userLocation.lng]);
      updateLeafletPickupMarker(state.userLocation.lat, state.userLocation.lng);
    } else {
      toast("📍 Getting your GPS location...");
      startGPS();
    }
  },

  dropInputChanged: async function() {
    const val = document.getElementById("drop-input").value.trim();
    const sug = document.getElementById("drop-suggestions");
    if(val.length<2){sug.style.display="none";return;}
    clearTimeout(_deb);_deb=setTimeout(async()=>{
      const results=await fetchPlaceSuggestions(val);
      if(!results.length){sug.style.display="none";return;}
      sug.style.display="block";
      sug.innerHTML=results.map(p=>`<div class="suggestion-item" onclick="App.selectDropSugg('${encodeURIComponent(p.display_name)}','${p.lat}','${p.lon}')">📍 ${p.display_name.substring(0,60)}</div>`).join("");
    },250);
  },

  selectDropSugg: function(name, lat, lng) {
    const decoded = decodeURIComponent(name);
    document.getElementById("drop-input").value = decoded;
    document.getElementById("drop-suggestions").style.display = "none";
    state.dropLocation = { address: decoded, lat: parseFloat(lat), lng: parseFloat(lng) };
    updateLeafletDropMarker(parseFloat(lat), parseFloat(lng));
    calcRouteAndFare();
  },

  calcRoute: function() {
    const dropVal = document.getElementById("drop-input").value.trim();
    if (!dropVal) return toast("Enter a drop location first");
    if (state.dropLocation?.lat) {
      calcRouteAndFare();
    } else {
      toast("Select a drop location from suggestions first");
    }
  },

  selectVehicle: function(el, name) {
    document.querySelectorAll(".vehicle-option").forEach(v => v.classList.remove("selected"));
    el.classList.add("selected");
    state.selectedVehicle = { name, ...VEHICLE_RATES[name] };
    if (state.dropLocation?.lat && state.userLocation.lat) calcRouteAndFare();
  },

  selectPayment: function(method) {
    state.selectedPayment = method;
    document.querySelectorAll(".payment-opt").forEach(p => p.classList.remove("selected"));
    document.getElementById(`pay-${method}`).classList.add("selected");
    const qrSec = document.getElementById("upi-qr-section");
    if (qrSec) qrSec.style.display = (method === "upi") ? "block" : "none";
  },

  // ─── PAYMENT QR ─────────────────────────────────────────────────────────────
  showPaymentQR: function(amount) {
    state.currentQRAmount = amount || state.estimatedFare || 0;
    document.getElementById("qr-modal-img").src = QR_IMAGE_B64;
    document.getElementById("qr-upi-display").textContent = `UPI: ${ADMIN_UPI}`;
    document.getElementById("qr-amount-display").textContent = amount > 0 ? `Amount: ₹${amount}` : `Estimated: ₹${state.estimatedFare}`;
    document.getElementById("qr-modal").style.display = "flex";
  },

  getQRAmount: function() { return state.currentQRAmount || state.estimatedFare || 0; },

  closeQR: function() { document.getElementById("qr-modal").style.display = "none"; },
  closeQRIfBg: function(e) { if (e.target.id === "qr-modal") App.closeQR(); },

  // ─── BOOK RIDE ─────────────────────────────────────────────────────────────
  bookRide: async function() {
    const pickup = document.getElementById("pickup-input").value.trim();
    const drop   = document.getElementById("drop-input").value.trim();
    const name   = document.getElementById("user-name-input").value.trim();
    const phone  = document.getElementById("user-phone-input").value.trim();
    const note   = document.getElementById("user-note").value.trim();
    const otherName = document.getElementById("other-name")?.value.trim()||"";
    const otherPhone = document.getElementById("other-phone")?.value.trim()||"";
    const schedTime = document.getElementById("sched-time")?.value||"";
    const rideFor = otherName ? "other" : "self";

    if (!pickup) return toast("📍 Enter your pickup location");
    if (!drop)   return toast("🏁 Please enter drop location");
    if (!name)   return toast("👤 Please enter your name");
    if (!phone || phone.length < 10) return toast("📞 Please enter a valid phone number");

    if (!state.estimatedFare && state.userLocation.lat && state.dropLocation?.lat) {
      await calcRouteAndFare();
      await new Promise(r => setTimeout(r, 1000));
    }

    const v    = state.selectedVehicle;
    const fare = state.estimatedFare || (v.base + 5 * v.perKm);
    const km   = state.estimatedKm   || 5;

    const btn = document.getElementById("book-btn");
    btn.innerHTML = '<span class="spinner"></span> Booking...';
    btn.disabled  = true;

    try {
      const rideRef = await addDoc(collection(db, "rides"), {
        userName: name, userPhone: phone, userNote: note,
        vehicle: v.name, vehicleIcon: v.icon,
        pickup, drop,
        pickupLat: state.userLocation.lat || null,
        pickupLng: state.userLocation.lng || null,
        dropLat: state.dropLocation?.lat || null,
        dropLng: state.dropLocation?.lng || null,
        fare, estimatedKm: km,
        paymentMethod: state.selectedPayment,
        status: schedTime ? "scheduled" : "pending",
        createdAt: new Date().toISOString(),
        scheduledFor: schedTime||null,
        rideFor: rideFor,
        passengerName: otherName||name,
        passengerPhone: otherPhone||phone,
        driverEmail: null, driverUid: null,
        driverName: null, driverPhone: null,
        rating: null, feedback: null,
        messages: []
      });

      state.currentRideId = rideRef.id;
      state.userProfile = { name, phone };
      localStorage.setItem("ridego_profile", JSON.stringify(state.userProfile));

      // Show QR for UPI payments immediately
      if (state.selectedPayment === "upi") {
        App.showPaymentQR(fare);
      }

      showScreen("screen-searching");
      document.getElementById("search-booking-summary").innerHTML = `
        <div class="card">
          <p><b>${v.icon} ${v.name}</b></p>
          <p style="font-size:22px;font-weight:900;color:var(--primary)">₹${fare}</p>
          <p style="font-size:13px;color:var(--muted)">📍 ${pickup}</p>
          <p style="font-size:13px;color:var(--muted)">🏁 ${drop}</p>
          <p style="font-size:13px;color:var(--muted)">~${km.toFixed(1)} km · 💳 ${state.selectedPayment.toUpperCase()}</p>
          ${state.selectedPayment==="upi"?`<button class="btn btn-green btn-sm" onclick="App.showPaymentQR(${fare})">📱 Show Payment QR</button>`:""}
        </div>`;
      App.watchRideStatus(rideRef.id, fare, { pickup, drop });
      toast("✅ Ride booked! Finding driver...");
    } catch (err) {
      toast("❌ Booking failed: " + (err.message || "Check Firestore rules"));
    }
    btn.innerHTML = "🚀 Book Now";
    btn.disabled  = false;
  },

  watchRideStatus: function(rideId, fare, rideInfo) {
    if (state.activeRideSub) state.activeRideSub();
    let attempt = 1; const MAX_TRIES = 3;
    let retryTimer = null;
    const scheduleRetry = () => {
      retryTimer = setTimeout(async () => {
        if (attempt >= MAX_TRIES) {
          toast("❌ No drivers available. Try again.");
          if (state.activeRideSub) state.activeRideSub();
          try { await updateDoc(doc(db,"rides",rideId),{status:"cancelled"}); } catch(_) {}
          showScreen("screen-user-booking"); return;
        }
        attempt++;
        const se = document.getElementById("search-status-text");
        if (se) se.textContent = `🔄 Retry ${attempt}/${MAX_TRIES}...`;
        scheduleRetry();
      }, 60000);
    };
    scheduleRetry();

    state.activeRideSub = onSnapshot(doc(db,"rides",rideId), snap => {
      if (!snap.exists()) return;
      const data = snap.data();

      if (data.driverLat && data.driverLng &&
          document.getElementById("screen-ride-active")?.classList.contains("active")) {
        updateActiveMapDriver(data.driverLat, data.driverLng, data.pickupLat, data.pickupLng);
      }

      if (data.status === "accepted" || data.status === "arriving" || data.status === "pickedup") {
        clearTimeout(retryTimer);
        if (data.status === "accepted") {
          sendLocalNotif("🏍️ Driver Found!", `${data.driverName} · 📞 ${data.driverPhone}`);
        }
        if (!document.getElementById("screen-ride-active")?.classList.contains("active")) {
          showScreen("screen-ride-active");
          setTimeout(() => initActiveMap(data.driverLat || data.pickupLat || 17.9689, data.driverLng || data.pickupLng || 79.5941), 300);
        }
        App.renderActiveRide(rideId, data, rideInfo);
      } else if (data.status === "completed") {
        clearTimeout(retryTimer);
        sendLocalNotif("✅ Ride Complete", `₹${data.fare}`);
        if (state.activeRideSub) state.activeRideSub();
        showScreen("screen-ride-done");
        state.pendingRating = { rideId, driverUid: data.driverUid, driverName: data.driverName };
        document.getElementById("ride-done-summary").innerHTML = `
          <div class="fare-box">
            <div class="fare-label">Total Fare</div>
            <div class="fare-amount">₹${data.fare}</div>
            <div style="font-size:13px;color:var(--muted);margin-top:4px">${(data.paymentMethod||"CASH").toUpperCase()}</div>
          </div>
          <p style="font-size:13px;margin-top:10px">🚗 Driver: <b>${data.driverName||"—"}</b></p>
          <p style="font-size:13px">📞 ${data.driverPhone||""}</p>
          <p style="font-size:13px">📍 ${data.pickup} → ${data.drop}</p>
          <p style="font-size:13px">📏 ~${data.estimatedKm?.toFixed(1)||"—"} km</p>
          ${data.paymentMethod==="upi"?`<button class="btn btn-green" style="margin-top:10px" onclick="App.showPaymentQR(${data.fare})">📱 Pay ₹${data.fare} via UPI</button>`:""}
        `;
      } else if (data.status === "cancelled") {
        clearTimeout(retryTimer);
        toast("❌ Ride was cancelled.");
        showScreen("screen-user-booking");
      }
    });
  },

  cancelSearch: async function() {
    if (state.currentRideId) await updateDoc(doc(db,"rides",state.currentRideId), { status:"cancelled" });
    if (state.activeRideSub) state.activeRideSub();
    showScreen("screen-user-booking");
  },

  renderActiveRide: function(rideId, data, rideInfo) {
    if (data.driverLat) updateActiveMapDriver(data.driverLat, data.driverLng, data.pickupLat, data.pickupLng);

    const isPickedUp = data.status === "pickedup" || data.status === "arriving";

    document.getElementById("driver-info-section").innerHTML = `
      <div class="driver-found-banner">✅ Driver Confirmed! Details below — call or chat anytime</div>
      <div class="driver-info-card">
        <div style="display:flex;align-items:center;gap:14px">
          <div class="driver-avatar">${data.vehicleIcon||"🚗"}</div>
          <div style="flex:1">
            <div class="d-name">${data.driverName||"Your Driver"}</div>
            <div class="d-detail">${data.vehicle} · ${data.driverVehicleNum||""}</div>
            <div class="d-rating">★ ${data.driverRating||"New"} · <span class="live-dot"></span>Live</div>
          </div>
          <div style="text-align:right">
            <div style="font-size:22px;font-weight:900;color:var(--primary)">₹${data.fare}</div>
            <div style="font-size:11px;opacity:0.6">${(data.paymentMethod||"cash").toUpperCase()}</div>
          </div>
        </div>
        <div class="phone-strip">
          <div><div class="ph-label">📞 DRIVER PHONE</div><div style="font-size:17px;font-weight:900">${data.driverPhone||"—"}</div></div>
          <div><span class="phone-pill" onclick="window.open('tel:${data.driverPhone}')">📞 Call</span><span class="phone-pill" onclick="window.open('https://wa.me/91${data.driverPhone}?text=Hi, RideGo passenger here.')">WA</span></div>
        </div>
        <div class="contact-row">
          <button class="contact-btn" style="background:var(--primary)" onclick="App.openChat('${rideId}','user','${data.driverName||"Driver"}','${data.driverPhone}')">💬 Chat Driver</button>
          <button class="contact-btn" onclick="App.shareTrip()">📤 Share</button>
          ${data.paymentMethod==="upi"?`<button class="contact-btn" onclick="App.showPaymentQR(${data.fare})">💳 Pay QR</button>`:""}
        </div>
      </div>`;

    document.getElementById("tracking-steps").innerHTML = `
      <div class="tracking-step">
        <div class="step-dot done">✓</div>
        <div class="step-info"><h4>Ride Booked</h4><p>Booking confirmed</p></div>
      </div>
      <div class="tracking-step">
        <div class="step-dot done">✓</div>
        <div class="step-info"><h4>Driver Assigned</h4><p>${data.driverName||"Driver"} accepted</p></div>
      </div>
      <div class="tracking-step">
        <div class="step-dot ${isPickedUp?'done':'active'}">${isPickedUp?'✓':'🏍'}</div>
        <div class="step-info"><h4>On the Way</h4><p>Driver heading to pickup</p></div>
      </div>
      <div class="tracking-step">
        <div class="step-dot ${isPickedUp?'active':'pending'}">${isPickedUp?'🏍':'🏁'}</div>
        <div class="step-info"><h4>${isPickedUp?"Ride in Progress":"Ride in Progress"}</h4><p>${isPickedUp?"Heading to destination":"Waiting for pickup"}</p></div>
      </div>`;

    document.getElementById("active-ride-actions").innerHTML = `
      <div style="display:flex;gap:8px">
        <button class="btn btn-gray" style="flex:1" onclick="App.cancelActiveRide('${rideId}')">Cancel Ride</button>
        <button class="btn btn-red" style="flex:1;width:auto" onclick="App.triggerSOS()">🆘 SOS</button>
      </div>
      <p style="font-size:12px;color:var(--muted);text-align:center;margin-top:8px">Driver location updates live</p>`;
  },

  cancelActiveRide: async function(rideId) {
    if (!confirm("Cancel this ride?")) return;
    await updateDoc(doc(db,"rides",rideId), { status:"cancelled" });
    if (state.activeRideSub) state.activeRideSub();
    showScreen("screen-user-booking");
    toast("Ride cancelled.");
  },

  triggerSOS: function() {
    const emer = JSON.parse(localStorage.getItem("ridego_emergency")||"{}");
    if (emer.phone) window.open(`tel:${emer.phone}`);
    else window.open("tel:112");
    toast("🆘 SOS activated!", 5000);
  },

  setRating: function(n) {
    state.currentRating = n;
    document.querySelectorAll("#star-rating span").forEach((s, i) => {
      s.textContent = i < n ? "★" : "☆";
      s.style.color  = i < n ? "#FFD600" : "#ccc";
    });
  },

  submitRating: async function() {
    if (!state.pendingRating || !state.currentRating) return toast("Please select a rating");
    try {
      await updateDoc(doc(db,"rides",state.pendingRating.rideId), {
        rating: state.currentRating,
        feedback: document.getElementById("ride-feedback").value
      });
      if (state.pendingRating.driverUid && state.pendingRating.driverUid !== "admin") {
        const dRef = doc(db,"users",state.pendingRating.driverUid);
        await runTransaction(db, async t => {
          const d = await t.get(dRef);
          if (!d.exists()) return;
          const sum = (d.data().sumRatings||0) + state.currentRating;
          const cnt = (d.data().totalRatings||0) + 1;
          t.update(dRef, { sumRatings: sum, totalRatings: cnt, avgRating: (sum/cnt).toFixed(1) });
        });
      }
      toast("⭐ Thanks for rating!");
      state.pendingRating = null;
      App.goUserBooking();
    } catch(e) { toast("❌ Failed: " + e.message); }
  },

  showUserRides: async function() {
    showScreen("screen-user-rides");
    const phone = state.userProfile?.phone || document.getElementById("user-phone-input")?.value;
    if (!phone) { document.getElementById("user-rides-list").innerHTML = "<div class='card'>Save your phone number to see ride history</div>"; return; }
    try {
      const q = query(collection(db,"rides"), where("userPhone","==",phone), orderBy("createdAt","desc"), limit(20));
      const snap = await getDocs(q);
      if (snap.empty) { document.getElementById("user-rides-list").innerHTML = "<div class='card' style='text-align:center;padding:30px'><div style='font-size:48px'>🚗</div><h3 style='margin:12px 0'>No Rides Yet</h3><p style='color:var(--muted)'>Book your first ride!</p></div>"; return; }
      let html = "";
      snap.forEach(d => {
        const r = d.data();
        html += `<div class="ride-card ${r.status}">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <h4>${r.vehicleIcon||"🚗"} ${r.vehicle}</h4>
            <span class="badge badge-${r.status}">${r.status}</span>
          </div>
          <div class="route-line">
            <div class="pickup">📍 ${r.pickup}</div>
            <div class="drop">🏁 ${r.drop}</div>
          </div>
          <div style="display:flex;justify-content:space-between;align-items:center">
            <span class="fare">₹${r.fare}</span>
            <span style="font-size:12px;color:var(--muted)">${new Date(r.createdAt).toLocaleDateString("en-IN",{day:"2-digit",month:"short",year:"numeric"})}</span>
          </div>
          ${r.driverName?`<div style="background:rgba(0,0,0,.04);border-radius:10px;padding:8px 10px;margin-top:8px"><div style="display:flex;justify-content:space-between;align-items:center"><span style="font-size:13px;font-weight:700">🚗 ${r.driverName}</span><span style="font-size:12px;color:var(--muted)">★ ${r.rating||"Not rated"}</span></div>${r.driverPhone?`<div style="margin-top:4px"><span class="phone-pill" onclick="window.open('tel:${r.driverPhone}')">📞 ${r.driverPhone}</span><span class="phone-pill" onclick="window.open('https://wa.me/91${r.driverPhone}')">WA</span></div>`:""}</div>`:""}
          ${(r.status==="accepted"||r.status==="pickedup"||r.status==="arriving")?`<button class="btn btn-primary btn-sm" style="margin-top:8px" onclick="App.reopenActiveRide('${d.id}')">👁 View Active Ride</button>`:""}
        </div>`;
      });
      document.getElementById("user-rides-list").innerHTML = html;
    } catch(e) { document.getElementById("user-rides-list").innerHTML = "<div class='card'>Could not load rides.</div>"; }
  },

  showUserProfile: function() {
    showScreen("screen-user-profile");
    const _sv=JSON.parse(localStorage.getItem("ridego_saved")||"{}");if(_sv.home){const e=document.getElementById("saved-home");if(e)e.value=_sv.home;}if(_sv.work){const e=document.getElementById("saved-work");if(e)e.value=_sv.work;}
    if (state.userProfile.name) {
      document.getElementById("profile-name").textContent = state.userProfile.name;
      document.getElementById("profile-name-input").value = state.userProfile.name;
    }
    if (state.userProfile.phone) document.getElementById("profile-phone-input").value = state.userProfile.phone;
    const emer = JSON.parse(localStorage.getItem("ridego_emergency")||"{}");
    if (emer.name)  document.getElementById("emer-name").value  = emer.name;
    if (emer.phone) document.getElementById("emer-phone").value = emer.phone;
  },

  saveUserProfile: function() {
    const name  = document.getElementById("profile-name-input").value.trim();
    const phone = document.getElementById("profile-phone-input").value.trim();
    if (!name||!phone) return toast("Enter name and phone");
    state.userProfile = { name, phone };
    localStorage.setItem("ridego_profile", JSON.stringify(state.userProfile));
    document.getElementById("profile-name").textContent = name;
    toast("✅ Profile saved!");
  },

  saveEmergencyContact: function() {
    const name  = document.getElementById("emer-name").value.trim();
    const phone = document.getElementById("emer-phone").value.trim();
    if (!name||!phone) return toast("Enter emergency contact details");
    localStorage.setItem("ridego_emergency", JSON.stringify({ name, phone }));
    toast("✅ Emergency contact saved!");
  },

  // ─── DRIVER LOGIN ──────────────────────────────────────────────────────────
  driverLogin: async function() {
    const email = document.getElementById("d-login-email").value.trim();
    const pass  = document.getElementById("d-login-pass").value;
    if (!email||!pass) return toast("Enter email and password");
    const btn = document.querySelector("#screen-driver-login .btn-primary");
    btn.innerHTML = '<span class="spinner"></span>'; btn.disabled = true;
    try {
      const cred = await signInWithEmailAndPassword(auth, email, pass);
      const snap = await getDoc(doc(db,"users",cred.user.uid));
      if (!snap.exists()) { await signOut(auth); btn.innerHTML="Login"; btn.disabled=false; return toast("❌ Account not found"); }
      const ud = snap.data();
      if (ud.role !== "driver") { await signOut(auth); btn.innerHTML="Login"; btn.disabled=false; return toast("❌ Not a driver account"); }
      state.currentUser = { ...ud, uid: cred.user.uid };
      state.role = "driver";
      App.loadDriverDashboard();
    } catch(err) { toast("❌ " + err.message.replace("Firebase:","").trim()); }
    btn.innerHTML = "Login"; btn.disabled = false;
  },

  // ─── DRIVER SIGNUP ─────────────────────────────────────────────────────────
  driverSignup: async function() {
    const name    = document.getElementById("s-name").value.trim();
    const email   = document.getElementById("s-email").value.trim();
    const phone   = document.getElementById("s-phone").value.trim();
    const pass    = document.getElementById("s-pass").value;
    const vehicle = document.getElementById("s-vehicle").value;
    const vnum    = document.getElementById("s-vnum").value.trim();
    const upi     = document.getElementById("s-upi").value.trim();
    const bankAcc = document.getElementById("s-bank-acc").value.trim();
    const ifsc    = document.getElementById("s-ifsc").value.trim();

    if (!name||!email||!phone||!pass) return toast("Fill all personal details");
    if (pass.length < 8) return toast("Password must be at least 8 characters");
    if (!vehicle) return toast("Select vehicle type");
    if (!vnum)    return toast("Enter vehicle number");

    const btn = document.getElementById("signup-btn");
    btn.innerHTML = '<span class="spinner"></span> Creating account...';
    btn.disabled  = true;

    try {
      const cred = await createUserWithEmailAndPassword(auth, email, pass);
      const trialUntil = new Date(Date.now() + 7*24*60*60*1000).toISOString();
      const docStatus = {
        licence:   state.uploadedDocs.licence   ? "pending_review" : "missing",
        rc:        state.uploadedDocs.rc        ? "pending_review" : "missing",
        aadhaar:   state.uploadedDocs.aadhaar   ? "pending_review" : "missing",
        bank:      state.uploadedDocs.bank      ? "pending_review" : "missing",
        insurance: state.uploadedDocs.insurance ? "pending_review" : "missing",
        selfie:    state.uploadedDocs.selfie    ? "pending_review" : "missing"
      };
      await setDoc(doc(db,"users",cred.user.uid), {
        uid: cred.user.uid, name, email, phone, role: "driver",
        vehicle, vehicleNum: vnum,
        upiId: upi||"", bankAccount: bankAcc||"", ifsc: ifsc||"",
        documents: state.uploadedDocs||{},
        docStatus,
        approvalStatus: "trial",
        trialUntil, kycDeadline: trialUntil,
        isOnDuty: false, earnings: 0, withdrawn: 0,
        totalRides: 0, totalRatings: 0, sumRatings: 0, avgRating: "New",
        pendingPlatformFee: 0,
        joinedAt: new Date().toISOString()
      });
      state.currentUser = {
        uid: cred.user.uid, name, email, phone, role: "driver",
        vehicle, vehicleNum: vnum, upiId: upi||"",
        approvalStatus: "trial", trialUntil, isOnDuty: false,
        avgRating: "New", totalRides: 0, docStatus
      };
      state.role = "driver";
      toast("🎉 Account created! Start driving immediately.");
      App.loadDriverDashboard();
    } catch(err) {
      toast("❌ " + err.message.replace("Firebase:","").trim());
    }
    btn.innerHTML = "🚀 Register & Start Driving Now →";
    btn.disabled  = false;
  },

  handleFileUpload: function(docType, input) {
    if (!input.files[0]) return;
    const file = input.files[0];
    if (file.size > 5*1024*1024) { toast("File too large. Max 5MB."); return; }
    const reader = new FileReader();
    reader.onload = e => {
      state.uploadedDocs[docType] = { name: file.name, type: file.type, uploadedAt: new Date().toISOString() };
      const statusEl  = document.getElementById(`status-${docType}`);
      const sectionEl = document.getElementById(`kyc-${docType}`);
      if (statusEl)  statusEl.textContent = `✅ ${file.name} — Uploaded`;
      if (sectionEl) sectionEl.classList.add("uploaded");
    };
    reader.readAsDataURL(file);
  },

  uploadDocForDriver: async function(docType, input) {
    if (!input.files[0] || !state.currentUser) return;
    const file = input.files[0];
    if (file.size > 5*1024*1024) { toast("File too large. Max 5MB."); return; }
    const reader = new FileReader();
    reader.onload = async e => {
      const docData = { name: file.name, type: file.type, uploadedAt: new Date().toISOString() };
      const updateObj = {};
      updateObj[`documents.${docType}`] = docData;
      updateObj[`docStatus.${docType}`] = "pending_review";
      await updateDoc(doc(db,"users",state.currentUser.uid), updateObj);
      toast(`✅ ${docType} uploaded! Admin will verify within 24 hours.`);
      App.driverNav("kyc");
    };
    reader.readAsDataURL(file);
  },

  // ─── DRIVER DASHBOARD ──────────────────────────────────────────────────────
  loadDriverDashboard: async function() {
    showScreen("screen-driver");
    const driver = state.currentUser;
    document.getElementById("driver-header-name").textContent = driver.name || driver.email;
    document.getElementById("driver-header-status").textContent = `${driver.vehicle} · ${driver.vehicleNum||""}`;

    const now = new Date();
    const isTrial = driver.approvalStatus === "trial";
    const isApproved = driver.approvalStatus === "approved";
    const isRejected = driver.approvalStatus === "rejected";
    const trialUntil = driver.trialUntil ? new Date(driver.trialUntil) : null;
    const trialExpired = isTrial && trialUntil && now > trialUntil;
    const daysLeft = trialUntil ? Math.max(0, Math.ceil((trialUntil-now)/(24*3600*1000))) : 0;
    const canDrive = isApproved || (isTrial && !trialExpired);

    const bannerEl = document.getElementById("driver-approval-banner");
    bannerEl.style.display = "block";

    if (isTrial && !trialExpired) {
      bannerEl.innerHTML = `<div class="trial-banner" style="margin:10px">
        <h4>⚡ 7-Day Trial Active — ${daysLeft} day${daysLeft!==1?"s":""} remaining</h4>
        <p>Upload documents within ${daysLeft} days. Admin can also approve you <b>without documents</b>.</p>
        <button class="btn btn-blue btn-sm" style="margin-top:8px" onclick="App.driverNav('kyc')">📄 Upload Documents</button>
      </div>`;
    } else if (trialExpired) {
      bannerEl.innerHTML = `<div class="approval-banner"><h4>⏱ Trial Period Expired</h4>
        <p>Upload documents or contact admin for approval without documents.</p>
        <button class="btn btn-outline btn-sm" style="margin-top:8px" onclick="App.driverNav('kyc')">📄 Upload Documents</button>
      </div>`;
    } else if (isRejected) {
      bannerEl.innerHTML = `<div class="approval-banner"><h4>❌ Account Suspended</h4>
        <p>Contact admin: <a href="https://wa.me/${ADMIN_PHONE}" style="color:var(--primary)">WhatsApp Admin</a></p>
        <button class="btn btn-outline btn-sm" style="margin-top:8px" onclick="App.driverNav('kyc')">Re-upload Documents</button>
      </div>`;
    } else if (isApproved) {
      bannerEl.innerHTML = `<div class="free-badge" style="margin:10px">
        <div class="fb-icon">✅</div>
        <div><h4>Account Verified & Active</h4><p>You can accept rides and earn!</p></div>
      </div>`;
    }

    // Check pending platform fees
    const hasOverdueFees = await checkDriverPendingFees();
    const feeWarningEl = document.getElementById("driver-fee-warning");
    if (hasOverdueFees) {
      feeWarningEl.style.display = "block";
      feeWarningEl.innerHTML = `<div class="fee-warning">
        <h4>⚠️ Platform Fee Overdue!</h4>
        <p>You have unpaid platform fees (20% of cash rides). Pay within 24hrs or you'll stop receiving orders.</p>
        <button class="btn btn-primary btn-sm" style="margin-top:8px" onclick="App.driverNav('fees')">💳 Pay Now</button>
      </div>`;
    } else {
      feeWarningEl.style.display = "none";
    }

    const dutySection = document.getElementById("duty-toggle-section");
    // Drivers with overdue fees can't go online
    if (hasOverdueFees) {
      dutySection.style.display = "none";
    } else {
      dutySection.style.display = canDrive ? "flex" : "none";
    }

    if (!canDrive && !isApproved && !isTrial) {
      document.getElementById("driver-main-content").innerHTML = `
        <div class="card" style="text-align:center;padding:30px">
          <div style="font-size:48px">🚫</div>
          <h3 style="margin:12px 0">Account Suspended</h3>
          <p style="color:var(--muted)">Upload required documents or contact admin.</p>
          <button class="btn btn-primary" style="margin-top:12px" onclick="App.driverNav('kyc')">Upload Documents</button>
          <a href="https://wa.me/${ADMIN_PHONE}" class="btn btn-green" style="margin-top:6px;display:block;text-decoration:none;text-align:center">WhatsApp Admin</a>
        </div>`;
    } else {
      App.driverNav("rides");
    }
  },

  driverNav: async function(tab) {
    ["rides","earnings","kyc","withdraw","profile","fees"].forEach(t => {
      const el = document.getElementById(`dnav-${t}`);
      if (el) el.classList.remove("active");
    });
    const active = document.getElementById(`dnav-${tab}`);
    if (active) active.classList.add("active");
    const content = document.getElementById("driver-main-content");
    if (tab === "rides")    App.driverLoadRides(content);
    else if (tab === "earnings") App.driverLoadEarnings(content);
    else if (tab === "kyc")     App.driverLoadKYC(content);
    else if (tab === "withdraw") App.driverLoadWithdraw(content);
    else if (tab === "profile")  App.driverLoadProfile(content);
    else if (tab === "fees")     App.driverLoadFees(content);
  },

  driverLoadRides: function(content) {
    if (!state.currentUser) return;
    const isOnDuty = state.driverDuty;
    if (!isOnDuty) {
      content.innerHTML = `
        <div class="card" style="text-align:center;padding:30px">
          <div style="font-size:48px">😴</div>
          <h3 style="margin:12px 0">You're Offline</h3>
          <p style="color:var(--muted)">Toggle the switch above to go online</p>
        </div>
        <div class="section-label">Recent Rides</div>
        <div id="driver-ride-history"></div>`;
      App.loadDriverRideHistory();
      return;
    }
    content.innerHTML = `
      <div class="card" style="text-align:center;background:linear-gradient(135deg,#e8f5e9,#c8e6c9);margin:10px">
        <div style="font-size:32px">🟢</div>
        <h3 style="color:var(--green-dark)">You're Online!</h3>
        <p style="font-size:13px;color:var(--green-dark)">Searching for ride requests near you...</p>
        <div style="margin-top:8px"><div class="search-ring" style="width:40px;height:40px;border-width:3px;border-top-color:var(--green)"></div></div>
      </div>
      <div id="driver-available-rides"></div>
      <div class="section-label">Recent Rides</div>
      <div id="driver-ride-history"></div>`;
    App.listenForAvailableRides();
    App.loadDriverRideHistory();
  },

  listenForAvailableRides: async function() {
    if (state.driverRidesSub) state.driverRidesSub();
    const hasOverdueFees = await checkDriverPendingFees();
    if (hasOverdueFees) {
      const container = document.getElementById("driver-available-rides");
      if (container) container.innerHTML = `<div class="fee-warning" style="margin:10px"><h4>⚠️ Platform Fee Overdue</h4><p>Pay overdue fees to receive orders.</p><button class="btn btn-primary btn-sm" style="margin-top:8px" onclick="App.driverNav('fees')">💳 Pay Now</button></div>`;
      return;
    }
    const vehicle = state.currentUser.vehicle;
    const RADIUS_KM = 5; // 4-6km radius
    const q = query(collection(db,"rides"), where("status","==","pending"), where("vehicle","==",vehicle));
    state.driverRidesSub = onSnapshot(q, snap => {
      const container = document.getElementById("driver-available-rides");
      if (!container) return;
      let rides = snap.docs.map(d => ({ id: d.id, ...d.data() }));
      // Filter by 4-6km radius if driver has GPS
      if (state.userLocation.lat && rides.length) {
        rides = rides.filter(r => {
          if (!r.pickupLat) return true; // no coords, show anyway
          const dist = haversineKm(state.userLocation.lat, state.userLocation.lng, r.pickupLat, r.pickupLng);
          return dist <= RADIUS_KM;
        });
      }
      if (!rides.length) {
        container.innerHTML = `<div style="text-align:center;padding:20px;color:var(--muted);font-size:14px">🔍 No rides within ${RADIUS_KM}km right now...</div>`;
        return;
      }
      let html = `<div class="section-label">New Ride Requests (${rides.length})</div>`;
      rides.forEach(ride => {
        const minutesAgo = Math.floor((Date.now()-new Date(ride.createdAt))/60000);
        html += `<div class="incoming-alert" id="alert-${ride.id}">
          <div style="display:flex;justify-content:space-between;align-items:center">
            <h3>${ride.vehicleIcon} New Ride! ${minutesAgo>0?`(${minutesAgo}m ago)`:""}</h3>
            <span style="font-size:22px;font-weight:900">₹${ride.fare}</span>
          </div>
          <div class="route-line" style="background:rgba(255,255,255,0.2);margin:10px 0">
            <div class="pickup" style="color:#fff">📍 ${ride.pickup}</div>
            <div class="drop" style="color:#fff">🏁 ${ride.drop}</div>
          </div>
          <div style="background:rgba(255,255,255,.2);border-radius:10px;padding:8px 12px;margin:8px 0;display:flex;align-items:center;justify-content:space-between">
            <div><div style="font-size:11px;font-weight:800;opacity:.9">📞 PASSENGER PHONE</div><div style="font-size:17px;font-weight:900">${ride.userPhone}</div></div>
            <div style="display:flex;gap:6px">
              <button style="background:#fff;color:var(--green-dark);border:none;border-radius:8px;padding:6px 10px;font-weight:800;font-family:'Nunito',sans-serif;font-size:12px;cursor:pointer" onclick="window.open('tel:${ride.userPhone}')">📞 Call</button>
              <button style="background:#25D366;color:#fff;border:none;border-radius:8px;padding:6px 10px;font-weight:800;font-family:'Nunito',sans-serif;font-size:12px;cursor:pointer" onclick="window.open('https://wa.me/91${ride.userPhone}?text=Hi, I am your RideGo driver.')">WA</button>
            </div>
          </div>
          <p style="font-size:13px;opacity:0.9">👤 ${ride.userName} · 📏 ~${ride.estimatedKm?.toFixed(1)||"?"}km</p>
          <p style="font-size:12px;opacity:0.8;margin:4px 0">💳 ${(ride.paymentMethod||"cash").toUpperCase()} ${ride.paymentMethod==="cash"?"— Pay 20% platform fee within 24hrs":""}</p>
          <div class="timer-bar"><div class="timer-fill" id="timer-${ride.id}" style="width:100%"></div></div>
          <div style="display:flex;gap:8px;margin-top:10px">
            <button class="btn btn-sm" style="flex:1;background:#fff;color:var(--green-dark);font-weight:900" onclick="App.acceptRide('${ride.id}')">✅ Accept</button>
            <button class="btn btn-sm" style="flex:1;background:rgba(255,255,255,0.2);color:#fff" onclick="App.declineRide('${ride.id}')">❌ Decline</button>
            <button class="btn btn-sm" style="background:rgba(255,255,255,0.2);color:#fff" onclick="window.open('tel:${ride.userPhone}')">📞</button>
          </div>
        </div>`;
      });
      container.innerHTML = html;
      App.playRingtone();
      rides.forEach(ride => App.startRideTimer(ride.id));
    });
  },

  startRideTimer: function(rideId) {
    let w = 100;
    const t = setInterval(() => {
      w -= 2;
      const el = document.getElementById(`timer-${rideId}`);
      if (el) el.style.width = w + "%";
      else clearInterval(t);
      if (w <= 0) clearInterval(t);
    }, 600);
  },

  playRingtone: function() {
    try {
      const ctx = new (window.AudioContext || window.webkitAudioContext)();
      const notes = [523,659,784,659,784,880,784,659,523,659,784,523];
      let t = ctx.currentTime;
      notes.forEach((freq, i) => {
        const osc = ctx.createOscillator();
        const gain = ctx.createGain();
        osc.connect(gain); gain.connect(ctx.destination);
        osc.frequency.setValueAtTime(freq, t);
        osc.type = 'sine';
        gain.gain.setValueAtTime(0.4, t);
        gain.gain.exponentialRampToValueAtTime(0.01, t + 0.25);
        osc.start(t); osc.stop(t + 0.28);
        t += 0.22;
      });
      // Repeat 3 times for 60 seconds alert
      state._ringtoneInterval = setInterval(() => {
        const ctx2 = new (window.AudioContext || window.webkitAudioContext)();
        let t2 = ctx2.currentTime;
        [523,659,784,523].forEach((freq) => {
          const o = ctx2.createOscillator(), g = ctx2.createGain();
          o.connect(g); g.connect(ctx2.destination);
          o.frequency.value = freq; o.type = 'sine';
          g.gain.setValueAtTime(0.35, t2);
          g.gain.exponentialRampToValueAtTime(0.01, t2+0.3);
          o.start(t2); o.stop(t2+0.32); t2+=0.25;
        });
      }, 3000);
      setTimeout(() => { clearInterval(state._ringtoneInterval); }, 60000);
    } catch(e) {}
  },

  stopRingtone: function() {
    clearInterval(state._ringtoneInterval);
    state._ringtoneInterval = null;
  },

  acceptRide: async function(rideId) {
    if (!state.currentUser) return toast("❌ Not logged in");
    const driver = state.currentUser;
    try {
      await runTransaction(db, async t => {
        const rideRef = doc(db,"rides",rideId);
        const ride = await t.get(rideRef);
        if (!ride.exists()||ride.data().status!=="pending") throw new Error("Ride no longer available");
        t.update(rideRef, {
          status: "accepted",
          driverUid: driver.uid,
          driverEmail: driver.email,
          driverName: driver.name,
          driverPhone: driver.phone,
          driverVehicleNum: driver.vehicleNum||"",
          driverRating: driver.avgRating||"New",
          driverLat: state.userLocation.lat||null,
          driverLng: state.userLocation.lng||null,
          acceptedAt: new Date().toISOString()
        });
      });
      App.stopRingtone();
      state.driverActiveRide = rideId;
      if (state.driverRidesSub) state.driverRidesSub();
      toast("✅ Ride accepted! Head to pickup.");
      sendLocalNotif("✅ Ride Accepted!", "Head to pickup location.");
      App.showDriverActiveRide(rideId);
    } catch(err) { toast("❌ " + err.message); }
  },

  declineRide: function(rideId) {
    const el = document.getElementById(`alert-${rideId}`);
    if (el) el.style.display = "none";
    toast("Ride declined.");
  },

  showDriverActiveRide: function(rideId) {
    showScreen("screen-driver-ride");
    setTimeout(() => initDriverMap(state.userLocation.lat||17.9689, state.userLocation.lng||79.5941), 300);

    onSnapshot(doc(db,"rides",rideId), snap => {
      if (!snap.exists()) return;
      const ride = snap.data();
      if (ride.pickupLat) showDriverRouteOnMap(ride.pickupLat, ride.pickupLng, state.userLocation.lat||17.9689, state.userLocation.lng||79.5941);

      document.getElementById("dride-status-title").textContent =
        ride.status==="accepted" ? "Heading to Pickup" :
        ride.status==="arriving" ? "Arrived at Pickup" :
        ride.status==="pickedup" ? "Ride in Progress" : "Ride Done";

      document.getElementById("driver-ride-detail").innerHTML = `
        <div class="driver-info-card" style="margin:10px">
          <div style="display:flex;align-items:center;gap:12px">
            <div class="driver-avatar">👤</div>
            <div style="flex:1">
              <div class="d-name">${ride.userName}</div>
              <div class="d-detail">Passenger</div>
              <div style="margin-top:4px">
                <span style="font-size:13px;background:#e8f5e9;color:var(--green-dark);border-radius:8px;padding:3px 8px;font-weight:700">📞 ${ride.userPhone}</span>
              </div>
            </div>
            <div style="text-align:right">
              <div style="font-size:22px;font-weight:900;color:var(--primary)">₹${ride.fare}</div>
              <div style="font-size:11px;opacity:0.7">${(ride.paymentMethod||"cash").toUpperCase()}</div>
            </div>
          </div>
          <div class="route-line" style="margin-top:12px">
            <div class="pickup">📍 ${ride.pickup}</div>
            <div class="drop">🏁 ${ride.drop}</div>
          </div>
          ${ride.pickupLat?`<div style="margin:8px 0"><a href="https://www.google.com/maps/dir/?api=1&destination=${ride.pickupLat},${ride.pickupLng}&travelmode=driving" target="_blank" class="btn btn-blue btn-sm">🗺 Open in Google Maps</a></div>`:""}
          <div class="contact-row">
            <button class="contact-btn" onclick="window.open('tel:${ride.userPhone}')">📞 Call User</button>
            <button class="contact-btn" onclick="App.openChat('${rideId}','driver','${ride.userName}','${ride.userPhone}')">💬 Chat</button>
            <button class="contact-btn" onclick="window.open('https://wa.me/91${ride.userPhone}?text=Hi, I am your RideGo driver. On my way!')">📱 WhatsApp</button>
          </div>
        </div>`;

      const actions = document.getElementById("driver-ride-actions");
      if (ride.status === "accepted") {
        actions.innerHTML = `
          <p style="font-size:14px;color:var(--muted);margin-bottom:10px">Have you reached the pickup location?</p>
          <button class="btn btn-green" onclick="App.driverMarkArrived('${rideId}')">📍 I've Arrived at Pickup</button>
          <button class="btn btn-gray" onclick="App.cancelDriverRide('${rideId}')">Cancel Ride</button>`;
      } else if (ride.status === "arriving") {
        actions.innerHTML = `
          <p style="font-size:14px;color:var(--green-dark);font-weight:800;margin-bottom:10px">📍 You've arrived! Waiting for passenger.</p>
          <button class="btn btn-primary" onclick="App.driverPickupPassenger('${rideId}')">🏍 Passenger Boarded — Start Ride</button>
          <button class="btn btn-gray" onclick="App.cancelDriverRide('${rideId}')">Cancel Ride</button>`;
      } else if (ride.status === "pickedup") {
        actions.innerHTML = `
          <p style="font-size:14px;color:var(--muted);margin-bottom:10px">Ride in progress. Mark complete at destination.</p>
          <button class="btn btn-primary" onclick="App.driverCompleteRide('${rideId}','${ride.fare}','${ride.driverUid}','${ride.paymentMethod||"cash"}')">✅ Complete Ride — ₹${ride.fare}</button>
          <button class="btn btn-red btn-sm" onclick="App.triggerSOS()">🆘 SOS</button>`;
      } else if (ride.status === "completed") {
        const driverEarning = Math.round(parseInt(ride.fare) * 0.80);
        const platformFee = Math.round(parseInt(ride.fare) * 0.20);
        actions.innerHTML = `
          <div class="free-badge"><div class="fb-icon">💰</div>
            <div><h4>Ride Completed! ₹${driverEarning} earned</h4>
            <p>${ride.paymentMethod==="cash"?`Pay platform fee ₹${platformFee} within 24hrs`:"Platform fee auto-deducted"}</p>
          </div></div>
          ${ride.paymentMethod==="cash"?`<button class="btn btn-green" onclick="App.driverNav('fees')">💳 Pay Platform Fee ₹${platformFee}</button>`:""}
          <button class="btn btn-primary" onclick="App.loadDriverDashboard()">← Back to Dashboard</button>`;
      }
    });
  },

  driverMarkArrived: async function(rideId) {
    await updateDoc(doc(db,"rides",rideId), { status:"arriving", arrivedAt: new Date().toISOString() });
    toast("✅ Marked as arrived! User notified.");
  },

  driverPickupPassenger: async function(rideId) {
    await updateDoc(doc(db,"rides",rideId), { status:"pickedup", pickedUpAt: new Date().toISOString() });
    toast("🏍 Ride started!");
  },

  driverCompleteRide: async function(rideId, fare, driverUid, paymentMethod) {
    const fareAmt = parseInt(fare);
    const driverEarning = Math.round(fareAmt * 0.80);
    const platformFee = Math.round(fareAmt * 0.20);
    if (!confirm(`Complete ride? Collect ₹${fareAmt}. ${paymentMethod==="cash"?`You'll owe platform fee ₹${platformFee} within 24hrs.`:""}`)) return;
    await updateDoc(doc(db,"rides",rideId), { status:"completed", completedAt: new Date().toISOString() });
    if (driverUid && driverUid !== "admin") {
      await updateDoc(doc(db,"users",driverUid), {
        earnings: increment(driverEarning), totalRides: increment(1)
      });
      // For cash rides, create a pending platform fee record
      if (paymentMethod === "cash") {
        const dueBy = new Date(Date.now() + PLATFORM_FEE_DEADLINE_HRS * 3600000).toISOString();
        await addDoc(collection(db,"platformFees"), {
          driverUid, driverName: state.currentUser?.name||"", driverPhone: state.currentUser?.phone||"",
          rideId, amount: platformFee, fareAmount: fareAmt, paid: false,
          dueBy, createdAt: new Date().toISOString()
        });
        toast(`🎉 Ride done! ₹${driverEarning} earned. Pay ₹${platformFee} platform fee within 24hrs.`, 5000);
      } else {
        toast(`🎉 Ride done! ₹${driverEarning} earned.`);
      }
    }
    if (state.driverRidesSub) state.driverRidesSub();
    state.driverActiveRide = null;
    App.loadDriverDashboard();
  },

  cancelDriverRide: async function(rideId) {
    if (!confirm("Cancel this ride?")) return;
    await updateDoc(doc(db,"rides",rideId), { status:"cancelled", cancelledBy:"driver" });
    toast("Ride cancelled.");
    state.driverActiveRide = null;
    App.loadDriverDashboard();
  },

  loadDriverRideHistory: async function() {
    if (!state.currentUser) return;
    try {
      const q = query(collection(db,"rides"), where("driverUid","==",state.currentUser.uid), orderBy("createdAt","desc"), limit(10));
      const snap = await getDocs(q);
      const el = document.getElementById("driver-ride-history");
      if (!el) return;
      if (snap.empty) { el.innerHTML = `<div style="text-align:center;padding:20px;color:var(--muted)">No ride history yet</div>`; return; }
      let html = "";
      snap.forEach(d => {
        const r = d.data();
        html += `<div class="ride-card ${r.status}">
          <div style="display:flex;justify-content:space-between">
            <h4>${r.vehicleIcon||"🚗"} ${r.userName}</h4>
            <span class="fare">₹${Math.round(r.fare*0.80)}</span>
          </div>
          <div class="route-line"><div class="pickup">📍 ${r.pickup}</div><div class="drop">🏁 ${r.drop}</div></div>
          <p style="font-size:12px;color:var(--muted)">${new Date(r.createdAt).toLocaleString("en-IN")}</p>
        </div>`;
      });
      el.innerHTML = html;
    } catch(e) {}
  },

  driverLoadEarnings: async function(content) {
    if (!state.currentUser) return;
    const snap = await getDoc(doc(db,"users",state.currentUser.uid));
    const d = snap.data()||{};
    const total = d.earnings||0;
    const withdrawn = d.withdrawn||0;
    const balance = total - withdrawn;
    content.innerHTML = `
      <div class="earnings-grid">
        <div class="earnings-item"><div class="amount">₹${total}</div><div class="label">Total Earned</div></div>
        <div class="earnings-item"><div class="amount">₹${balance}</div><div class="label">Available Balance</div></div>
        <div class="earnings-item"><div class="amount">₹${withdrawn}</div><div class="label">Withdrawn</div></div>
        <div class="earnings-item"><div class="amount">${d.totalRides||0}</div><div class="label">Total Rides</div></div>
      </div>
      <div class="card">
        <div class="card-title">💡 Earnings Info</div>
        <p style="font-size:13px;color:var(--muted)">You earn <b>80%</b> of each fare. 20% is platform fee. For <b>cash rides</b>, you collect full fare but must pay 20% to RideGo within 24 hours via UPI.</p>
        <div style="background:#e8f5e9;border-radius:10px;padding:12px;margin-top:10px">
          <p style="font-size:14px;font-weight:700;color:var(--green-dark)">⭐ Rating: ${d.avgRating||"New"} (${d.totalRatings||0} ratings)</p>
        </div>
        <div style="margin-top:10px;background:#fff3e0;border-radius:10px;padding:12px">
          <p style="font-size:13px;font-weight:700;color:#e65100">⚠️ Cash Ride Policy: Collect full fare from user → Pay 20% to RideGo within 24hrs via UPI: <b>${ADMIN_UPI}</b></p>
        </div>
      </div>`;
  },

  // Platform fee payment screen for drivers
  driverLoadFees: async function(content) {
    if (!state.currentUser) return;
    try {
      const q = query(collection(db,"platformFees"), where("driverUid","==",state.currentUser.uid), where("paid","==",false));
      const snap = await getDocs(q);
      let totalDue = 0;
      let feesHtml = "";
      const now = new Date();
      snap.forEach(d => {
        const f = d.data();
        totalDue += f.amount;
        const overdue = now > new Date(f.dueBy);
        feesHtml += `<div class="ride-card" style="border-left-color:${overdue?"var(--red)":"var(--yellow)"}">
          <div style="display:flex;justify-content:space-between">
            <h4>Platform Fee for Ride</h4>
            <span class="fare" style="color:${overdue?"var(--red)":"var(--primary)"}">₹${f.amount}</span>
          </div>
          <p>Ride Fare: ₹${f.fareAmount}</p>
          <p>Due By: ${new Date(f.dueBy).toLocaleString("en-IN")} ${overdue?"🔴 OVERDUE":""}</p>
          <p style="font-size:12px;color:var(--muted)">${new Date(f.createdAt).toLocaleString("en-IN")}</p>
          <button class="btn btn-green btn-sm" style="margin-top:8px" onclick="App.markFeePaid('${d.id}','${f.amount}')">✅ Mark as Paid to Admin</button>
        </div>`;
      });
      content.innerHTML = `
        <div class="card">
          <div class="card-title">💸 Platform Fees Due</div>
          <div style="font-size:28px;font-weight:900;color:var(--red);text-align:center;margin:10px 0">₹${totalDue}</div>
          <p style="font-size:13px;color:var(--muted);text-align:center">Pay to Admin UPI: <b>${ADMIN_UPI}</b></p>
          ${totalDue > 0 ? `<button class="btn btn-green" onclick="App.showPaymentQR(${totalDue})">📱 Pay ₹${totalDue} via QR/UPI</button>
          <button class="btn btn-blue" onclick="window.open('https://wa.me/${ADMIN_PHONE}?text=Hi Admin, I am ${state.currentUser.name} (${state.currentUser.phone}). I have paid platform fee of Rs.${totalDue}. Please confirm.')">📱 Notify Admin on WhatsApp</button>` : ""}
        </div>
        ${totalDue === 0 ? `<div class="free-badge" style="margin:10px"><div class="fb-icon">✅</div><div><h4>No Pending Fees</h4><p>You're all clear!</p></div></div>` : feesHtml}`;
    } catch(e) {
      content.innerHTML = `<div class="card"><p>Could not load fees. Check connection.</p></div>`;
    }
  },

  markFeePaid: async function(feeId, amount) {
    if (!confirm(`Mark ₹${amount} as paid to admin?`)) return;
    await updateDoc(doc(db,"platformFees",feeId), { paid: true, paidAt: new Date().toISOString(), paymentMethod: "driver_claimed" });
    toast("✅ Marked as paid! Admin will verify.");
    App.driverNav("fees");
  },

  driverLoadKYC: async function(content) {
    if (!state.currentUser) return;
    const snap = await getDoc(doc(db,"users",state.currentUser.uid));
    const d = snap.data()||{};
    const docStatus = d.docStatus||{};
    const trialUntil = d.trialUntil ? new Date(d.trialUntil) : null;
    const daysLeft = trialUntil ? Math.max(0, Math.ceil((trialUntil-new Date())/(24*3600*1000))) : 0;
    const isUrgent = daysLeft <= 2;
    const docs = [
      { key:"licence",   label:"Driving Licence",     icon:"🪪" },
      { key:"rc",        label:"Vehicle RC",           icon:"📋" },
      { key:"aadhaar",   label:"Aadhaar Card",         icon:"🪪" },
      { key:"bank",      label:"Bank/Passbook Cheque", icon:"🏦" },
      { key:"insurance", label:"Vehicle Insurance",    icon:"🛡️" },
      { key:"selfie",    label:"Profile Selfie",       icon:"🤳" }
    ];
    const statusIcon  = { "missing":"📤 Upload", "pending_review":"⏳ Pending Review", "verified":"✅ Verified", "rejected":"❌ Rejected" };
    const statusColor = { "missing":"var(--muted)", "pending_review":"var(--blue)", "verified":"var(--green-dark)", "rejected":"var(--red)" };
    let html = `
      <div style="background:#e3f2fd;border-radius:12px;padding:12px 14px;margin:10px;border:1px solid var(--blue)">
        <p style="font-size:14px;font-weight:800;color:#1565c0">ℹ️ Documents are optional!</p>
        <p style="font-size:13px;color:#1976d2;margin-top:4px">Admin can approve you without documents. <a href="https://wa.me/${ADMIN_PHONE}" style="color:var(--primary)">Contact Admin on WhatsApp</a> for quick approval.</p>
      </div>
      <div class="${isUrgent?'doc-deadline urgent':'doc-deadline'}" style="margin:10px">
        ⏰ Trial: <b>${daysLeft} day${daysLeft!==1?"s":""} remaining</b>${daysLeft===0?" — EXPIRED!":""}
      </div>
      <div class="card"><div class="card-title">📄 KYC Documents Status</div>`;
    docs.forEach(dc => {
      const st = docStatus[dc.key] || "missing";
      html += `<div class="doc-verify-row">
        <div class="doc-icon">${dc.icon}</div>
        <div class="doc-info">
          <h5>${dc.label}</h5>
          <p style="color:${statusColor[st]};font-weight:700">${statusIcon[st]||st}</p>
        </div>
        ${st==="missing"||st==="rejected"?`<label style="background:var(--primary);color:#fff;padding:6px 12px;border-radius:8px;font-size:12px;font-weight:700;cursor:pointer;text-transform:none">
          Upload<input type="file" accept="image/*,application/pdf" style="display:none" onchange="App.uploadDocForDriver('${dc.key}',this)">
        </label>`:""}
      </div>`;
    });
    html += `</div><div style="padding:0 10px 16px"><a href="https://wa.me/${ADMIN_PHONE}?text=Hi Admin, I am ${(d.name||"").replace(/'/g,"")} (${d.phone||""}), driver on RideGo. Please approve my account." class="btn btn-green" style="display:block;text-decoration:none;text-align:center">📱 Request Admin Approval (WhatsApp)</a></div>`;
    content.innerHTML = html;
  },

  driverLoadWithdraw: async function(content) {
    if (!state.currentUser) return;
    const snap = await getDoc(doc(db,"users",state.currentUser.uid));
    const d = snap.data()||{};
    const balance = (d.earnings||0)-(d.withdrawn||0);
    content.innerHTML = `
      <div class="earnings-grid">
        <div class="earnings-item" style="grid-column:1/-1"><div class="amount" style="font-size:32px">₹${balance}</div><div class="label">Available to Withdraw</div></div>
      </div>
      <div class="card">
        <div class="card-title">🏧 Request Withdrawal</div>
        ${balance < 100 ? `<div style="background:#fff3e0;border-radius:10px;padding:12px;color:#e65100;font-weight:700;margin-bottom:12px">Minimum withdrawal: ₹100</div>`:""}
        <label>Amount (Min ₹100)</label>
        <input id="withdraw-amount" type="number" placeholder="Enter amount" min="100" max="${balance}">
        <label>UPI ID / Bank Details</label>
        <input id="withdraw-upi" placeholder="UPI ID or account number" value="${d.upiId||""}">
        <label>Note (Optional)</label>
        <input id="withdraw-note" placeholder="Any note for admin">
        <button class="btn btn-green" onclick="App.requestWithdraw(${balance})" ${balance<100?"disabled":""}>Request Withdrawal →</button>
        <p style="font-size:12px;color:var(--muted);margin-top:8px;text-align:center">Processed within 24 hours on weekdays</p>
      </div>
      <div class="section-label">Recent Withdrawals</div>
      <div id="withdraw-history"></div>`;
    App.loadWithdrawHistory();
  },

  requestWithdraw: async function(balance) {
    const amount = parseInt(document.getElementById("withdraw-amount").value)||0;
    const upi    = document.getElementById("withdraw-upi").value.trim();
    const note   = document.getElementById("withdraw-note").value.trim();
    if (amount < 100) return toast("Minimum withdrawal ₹100");
    if (amount > balance) return toast("Insufficient balance");
    if (!upi) return toast("Enter UPI ID or bank details");
    try {
      await addDoc(collection(db,"withdrawals"), {
        driverUid: state.currentUser.uid, driverName: state.currentUser.name,
        driverPhone: state.currentUser.phone, amount, upiId: upi, note,
        status: "pending", requestedAt: new Date().toISOString()
      });
      toast("✅ Withdrawal requested! Admin will process within 24 hours.");
      App.driverNav("withdraw");
    } catch(e) { toast("❌ " + e.message); }
  },

  loadWithdrawHistory: async function() {
    if (!state.currentUser) return;
    try {
      const q = query(collection(db,"withdrawals"), where("driverUid","==",state.currentUser.uid), orderBy("requestedAt","desc"), limit(10));
      const snap = await getDocs(q);
      const el = document.getElementById("withdraw-history");
      if (!el) return;
      if (snap.empty) { el.innerHTML = "<div style='text-align:center;padding:16px;color:var(--muted)'>No withdrawal history</div>"; return; }
      let html = "";
      snap.forEach(d => {
        const w = d.data();
        html += `<div class="ride-card ${w.status}">
          <div style="display:flex;justify-content:space-between"><h4>₹${w.amount}</h4><span class="badge badge-${w.status==="paid"?"approved":w.status}">${w.status}</span></div>
          <p>UPI: ${w.upiId}</p>
          <p style="font-size:12px">${new Date(w.requestedAt).toLocaleString("en-IN")}</p>
        </div>`;
      });
      el.innerHTML = html;
    } catch(e) {}
  },

  driverLoadProfile: async function(content) {
    if (!state.currentUser) return;
    const d = state.currentUser;
    content.innerHTML = `
      <div class="card" style="text-align:center">
        <div style="font-size:56px">🏍️</div>
        <h3 style="margin-top:8px;font-size:18px;font-weight:800">${d.name}</h3>
        <p style="color:var(--muted)">${d.vehicle} · ${d.vehicleNum||""}</p>
        <p style="margin-top:4px">⭐ ${d.avgRating||"New"} · ${d.totalRides||0} rides</p>
      </div>
      <div class="card">
        <div class="card-title">Profile Details</div>
        <p><b>Email:</b> ${d.email}</p>
        <p style="margin-top:6px"><b>Phone:</b> ${d.phone}</p>
        <p style="margin-top:6px"><b>UPI:</b> ${d.upiId||"Not set"}</p>
        <p style="margin-top:6px"><b>Status:</b> <span class="badge badge-${d.approvalStatus}">${d.approvalStatus}</span></p>
        <a href="https://wa.me/${ADMIN_PHONE}" class="btn btn-green" style="display:block;text-decoration:none;text-align:center;margin-top:12px">📱 Contact Admin on WhatsApp</a>
        <button class="btn btn-outline" style="margin-top:8px" onclick="App.driverLogout()">Logout</button>
      </div>`;
  },

  toggleDuty: async function(checked) {
    state.driverDuty = checked;
    const label  = document.getElementById("duty-label");
    const sublbl = document.getElementById("duty-sublabel");
    if (checked) {
      label.textContent  = "You're Online 🟢";
      sublbl.textContent = "Accepting rides now";
      toast("🟢 You're now online!");
    } else {
      label.textContent  = "Go Online";
      sublbl.textContent = "Tap to start accepting rides";
      toast("⚫ You're offline.");
      if (state.driverRidesSub) { state.driverRidesSub(); state.driverRidesSub = null; }
    }
    if (state.currentUser) {
      await updateDoc(doc(db,"users",state.currentUser.uid), { isOnDuty: checked });
    }
    App.driverNav("rides");
  },

  driverLogout: async function() {
    if (state.driverRidesSub) state.driverRidesSub();
    if (state.currentUser) await updateDoc(doc(db,"users",state.currentUser.uid), { isOnDuty: false }).catch(()=>{});
    await signOut(auth);
    state.currentUser = null; state.role = null; state.driverDuty = false; state.driverActiveRide = null;
    showScreen("screen-landing");
    toast("👋 Logged out");
  },

  // ─── CHAT SYSTEM ───────────────────────────────────────────────────────────
  openChat: function(rideId, role, otherName, otherPhone) {
    state.chatRideId = rideId;
    state.chatRole   = role;
    document.getElementById("chat-title").textContent = `Chat with ${otherName}`;
    document.getElementById("chat-subtitle").textContent = `📞 ${otherPhone}`;
    document.getElementById("chat-messages").innerHTML = `<div style="text-align:center;padding:20px;color:var(--muted);font-size:13px">💬 Chat loaded</div>`;
    document.getElementById("chat-overlay").style.display = "flex";
    App.listenChat(rideId, role);
    document.getElementById("chat-input").focus();
  },

  listenChat: function(rideId, role) {
    if (state.chatSub) state.chatSub();
    state.chatSub = onSnapshot(doc(db,"rides",rideId), snap => {
      if (!snap.exists()) return;
      const messages = snap.data().messages || [];
      const container = document.getElementById("chat-messages");
      if (!container) return;
      if (!messages.length) { container.innerHTML = `<div style="text-align:center;padding:20px;color:var(--muted);font-size:13px">💬 Say hi!</div>`; return; }
      container.innerHTML = messages.map(m => {
        const isMine = m.sender === role;
        const time = new Date(m.timestamp).toLocaleTimeString("en-IN",{hour:"2-digit",minute:"2-digit"});
        return `<div class="chat-msg ${isMine?"sent":"received"}">${m.text}<div class="msg-time">${time}</div></div>`;
      }).join("");
      container.scrollTop = container.scrollHeight;
    });
  },

  sendChat: async function() {
    const input = document.getElementById("chat-input");
    const text  = input.value.trim();
    if (!text || !state.chatRideId) return;
    input.value = "";
    const message = { sender: state.chatRole, text, timestamp: new Date().toISOString() };
    try {
      const rideRef = doc(db,"rides",state.chatRideId);
      const snap = await getDoc(rideRef);
      const msgs = snap.data()?.messages || [];
      msgs.push(message);
      await updateDoc(rideRef, { messages: msgs });
    } catch(e) { toast("❌ Message failed"); }
  },

  sendQuick: function(text) { document.getElementById("chat-input").value = text; App.sendChat(); },
  closeChat: function() { document.getElementById("chat-overlay").style.display = "none"; if (state.chatSub) { state.chatSub(); state.chatSub = null; } },
  closeChatIfBg: function(e) { if (e.target.id === "chat-overlay") App.closeChat(); },

  // ─── ADMIN LOGIN ───────────────────────────────────────────────────────────
  adminLogin: async function() {
    const email = document.getElementById("a-login-email").value.trim();
    const pass  = document.getElementById("a-login-pass").value;
    if (!email||!pass) return toast("Enter email and password");
    const btn = document.getElementById("admin-login-btn");
    btn.innerHTML = '<span class="spinner"></span>'; btn.disabled = true;
    try {
      const cred = await signInWithEmailAndPassword(auth, email, pass);
      if (cred.user.email?.toLowerCase() !== ADMIN_EMAIL.toLowerCase()) {
        await signOut(auth); btn.innerHTML="Login as Admin"; btn.disabled=false;
        return toast("❌ Not an admin account");
      }
      const userRef = doc(db,"users",cred.user.uid);
      const snap = await getDoc(userRef);
      if (!snap.exists()) {
        await setDoc(userRef, { uid:cred.user.uid, email:cred.user.email, name:"Admin", role:"admin", createdAt: new Date().toISOString() });
      }
      state.currentUser = { uid:cred.user.uid, email:cred.user.email, name:"Admin", role:"admin" };
      state.role = "admin";
      App.loadAdminDashboard();
    } catch(err) { toast("❌ " + err.message.replace("Firebase:","").trim()); }
    btn.innerHTML = "Login as Admin"; btn.disabled = false;
  },

  loadAdminDashboard: async function() {
    showScreen("screen-admin");
    try {
      const [ridesSnap, usersSnap, withdrawSnap, feesSnap] = await Promise.all([
        getDocs(collection(db,"rides")),
        getDocs(query(collection(db,"users"),where("role","==","driver"))),
        getDocs(query(collection(db,"withdrawals"),where("status","==","pending"))),
        getDocs(query(collection(db,"platformFees"),where("paid","==",false)))
      ]);
      const rides = ridesSnap.docs.map(d=>d.data());
      const totalFare = rides.filter(r=>r.status==="completed").reduce((a,r)=>a+(r.fare||0),0);
      const pendingFeeAmt = feesSnap.docs.reduce((a,d)=>a+(d.data().amount||0),0);
      document.getElementById("admin-stats-grid").innerHTML = `
        <div class="stat-card"><div class="stat-num">${ridesSnap.size}</div><div class="stat-label">Total Rides</div></div>
        <div class="stat-card"><div class="stat-num">${usersSnap.size}</div><div class="stat-label">Drivers</div></div>
        <div class="stat-card"><div class="stat-num">₹${totalFare}</div><div class="stat-label">Total Revenue</div></div>
        <div class="stat-card"><div class="stat-num">${withdrawSnap.size}</div><div class="stat-label">Pending Payouts</div></div>
        <div class="stat-card"><div class="stat-num" style="color:var(--red)">₹${pendingFeeAmt}</div><div class="stat-label">Unpaid Fees</div></div>
        <div class="stat-card"><div class="stat-num">${feesSnap.size}</div><div class="stat-label">Fee Records</div></div>`;
    } catch(e) {
      document.getElementById("admin-stats-grid").innerHTML = "<div class='card' style='grid-column:1/-1;text-align:center'>Loading stats...</div>";
    }
    App.adminTab("rides");
  },

  adminTab: async function(tab) {
    document.querySelectorAll(".tab-btn").forEach(b=>b.classList.remove("active"));
    const tabMap = ["rides","kyc","drivers","withdrawals","fees","analytics"];
    const idx = tabMap.indexOf(tab);
    const allBtns = document.querySelectorAll(`#admin-tabs .tab-btn`);
    if (idx>=0 && allBtns[idx]) allBtns[idx].classList.add("active");
    const content = document.getElementById("admin-content");
    if (tab==="rides")       App.adminLoadRides(content);
    else if (tab==="kyc")         App.adminLoadKYC(content);
    else if (tab==="drivers")     App.adminLoadDrivers(content);
    else if (tab==="withdrawals") App.adminLoadWithdrawals(content);
    else if (tab==="fees")        App.adminLoadPlatformFees(content);
    else if (tab==="analytics")   App.adminLoadAnalytics(content);
  },

  adminLoadRides: function(content) {
    if (unsubAdmin) unsubAdmin();
    content.innerHTML = `<div class="tabs" style="padding:6px 10px">
      <button class="tab-btn active" onclick="App.adminFilterRides(this,'all')">All</button>
      <button class="tab-btn" onclick="App.adminFilterRides(this,'pending')">Pending</button>
      <button class="tab-btn" onclick="App.adminFilterRides(this,'accepted')">Active</button>
      <button class="tab-btn" onclick="App.adminFilterRides(this,'completed')">Done</button>
    </div><div id="admin-rides-list"></div>`;
    const q = query(collection(db,"rides"), orderBy("createdAt","desc"), limit(50));
    unsubAdmin = onSnapshot(q, snap => {
      let rides = snap.docs.map(d=>({id:d.id,...d.data()}));
      const activeFilter = document.getElementById("admin-rides-list")?.dataset?.filter || "all";
      if (activeFilter!=="all") rides = rides.filter(r=>r.status===activeFilter);
      const listEl = document.getElementById("admin-rides-list");
      if (!listEl) return;
      if (!rides.length) { listEl.innerHTML = "<div style='text-align:center;padding:30px;color:var(--muted)'>No rides found</div>"; return; }
      listEl.innerHTML = rides.map(r=>`<div class="ride-card ${r.status}">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <h4>${r.vehicleIcon||"🚗"} ${r.vehicle} — ${r.userName}</h4>
          <span class="badge badge-${r.status}">${r.status}</span>
        </div>
        <div class="route-line"><div class="pickup">📍 ${r.pickup}</div><div class="drop">🏁 ${r.drop}</div></div>
        <p>📞 ${r.userPhone} · 💰 ₹${r.fare} · 💳 ${(r.paymentMethod||"cash").toUpperCase()}</p>
        ${r.driverName?`<p>🚗 Driver: <b>${r.driverName}</b> (${r.driverPhone})</p>`:""}
        <div style="display:flex;gap:6px;margin-top:8px;flex-wrap:wrap">
          ${r.status==="pending"?`<button class="btn btn-green btn-sm" onclick="App.adminAcceptRide('${r.id}')">✅ Accept as Admin</button>`:""}
          ${r.status==="accepted"||r.status==="arriving"||r.status==="pickedup"?`<button class="btn btn-primary btn-sm" onclick="App.adminCompleteRide('${r.id}','${r.fare}','${r.driverUid||""}')">✅ Complete</button>`:""}
          ${r.status==="pending"||r.status==="accepted"||r.status==="arriving"?`<button class="btn btn-red btn-sm" onclick="App.adminCancelRide('${r.id}')">❌ Cancel</button>`:""}
          <button class="btn btn-sm btn-gray" onclick="window.open('tel:${r.userPhone}')">📞 User</button>
          ${r.driverPhone?`<button class="btn btn-sm btn-gray" onclick="window.open('tel:${r.driverPhone}')">📞 Driver</button>`:""}
        </div>
      </div>`).join("");
    });
  },

  adminFilterRides: function(btn, filter) {
    document.querySelectorAll("#admin-content .tabs .tab-btn").forEach(b=>b.classList.remove("active"));
    btn.classList.add("active");
    const el = document.getElementById("admin-rides-list");
    if (el) el.dataset.filter = filter;
    App.adminLoadRides(document.getElementById("admin-content"));
  },

  adminAcceptRide: async function(rideId) {
    try {
      await runTransaction(db, async t => {
        const rideRef = doc(db,"rides",rideId);
        const ride = await t.get(rideRef);
        if (!ride.exists()||ride.data().status!=="pending") throw new Error("Ride not available");
        t.update(rideRef, {
          status: "accepted", driverEmail: ADMIN_EMAIL, driverUid: "admin",
          driverName: "Admin Driver", driverPhone: "9110563922",
          driverVehicleNum: "ADMIN00", driverRating: "5.0",
          acceptedAt: new Date().toISOString()
        });
      });
      toast("✅ Ride accepted as Admin Driver");
    } catch(err) { toast("❌ " + err.message); }
  },

  adminCompleteRide: async function(rideId, fare, driverUid) {
    await updateDoc(doc(db,"rides",rideId), { status:"completed", completedAt: new Date().toISOString() });
    if (driverUid && driverUid!=="admin") {
      await updateDoc(doc(db,"users",driverUid), { earnings: increment(Math.round(fare*0.80)), totalRides: increment(1) });
    }
    toast("✅ Ride marked complete");
  },

  adminCancelRide: async function(rideId) {
    if (!confirm("Cancel this ride?")) return;
    await updateDoc(doc(db,"rides",rideId), { status:"cancelled", cancelledAt: new Date().toISOString() });
    toast("Ride cancelled.");
  },

  adminLoadKYC: async function(content) {
    const q = query(collection(db,"users"), where("role","==","driver"));
    const snap = await getDocs(q);
    let html = "<div class='section-label'>Driver KYC Approvals</div>";
    snap.forEach(d => {
      const u = d.data();
      const docStatus = u.docStatus||{};
      const allVerified = Object.values(docStatus).every(v=>v==="verified");
      const hasPending  = Object.values(docStatus).some(v=>v==="pending_review");
      html += `<div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div>
            <h4 style="font-size:15px;font-weight:800">${u.name}</h4>
            <p style="font-size:13px;color:var(--muted)">${u.vehicle} · ${u.vehicleNum||""} · ${u.phone}</p>
          </div>
          <span class="badge badge-${u.approvalStatus||"trial"}">${u.approvalStatus||"trial"}</span>
        </div>
        <div style="display:flex;gap:4px;flex-wrap:wrap;margin:8px 0">
          ${["licence","rc","aadhaar","bank","insurance","selfie"].map(k=>`
            <span style="font-size:11px;background:${docStatus[k]==="verified"?"#e8f5e9":docStatus[k]==="pending_review"?"#e3f2fd":docStatus[k]==="rejected"?"#ffebee":"#f5f5f5"};color:${docStatus[k]==="verified"?"var(--green-dark)":docStatus[k]==="pending_review"?"var(--blue)":docStatus[k]==="rejected"?"var(--red)":"var(--muted)"};padding:3px 8px;border-radius:20px;font-weight:700">${k}: ${docStatus[k]||"missing"}
              ${docStatus[k]==="pending_review"?`<span onclick="App.adminVerifyDoc('${d.id}','${k}')" style="cursor:pointer;text-decoration:underline">✓verify</span>`:""}
            </span>`).join("")}
        </div>
        <div style="display:flex;gap:6px;flex-wrap:wrap">
          <button class="btn btn-green btn-sm" onclick="App.adminApproveDriver('${d.id}')">✅ Approve (with/without docs)</button>
          <button class="btn btn-red btn-sm" onclick="App.adminRejectDriver('${d.id}')">❌ Reject</button>
          <button class="btn btn-gray btn-sm" onclick="window.open('tel:${u.phone}')">📞 Call</button>
          <a href="https://wa.me/91${u.phone}" class="btn btn-gray btn-sm" target="_blank">📱 WhatsApp</a>
        </div>
      </div>`;
    });
    content.innerHTML = snap.empty ? "<div style='text-align:center;padding:30px;color:var(--muted)'>No drivers yet</div>" : html;
  },

  adminApproveDriver: async function(uid) {
    if (!confirm("Approve this driver? They can drive immediately.")) return;
    const docStatus = { licence:"verified",rc:"verified",aadhaar:"verified",bank:"verified",insurance:"verified",selfie:"verified" };
    await updateDoc(doc(db,"users",uid), { approvalStatus:"approved", docStatus, approvedAt: new Date().toISOString() });
    toast("✅ Driver approved!");
    App.adminTab("kyc");
  },

  adminRejectDriver: async function(uid) {
    const reason = prompt("Rejection reason:");
    if (!reason) return;
    await updateDoc(doc(db,"users",uid), { approvalStatus:"rejected", rejectionReason: reason, rejectedAt: new Date().toISOString() });
    toast("Driver rejected.");
    App.adminTab("kyc");
  },

  adminVerifyDoc: async function(uid, docKey) {
    const updateObj = {};
    updateObj[`docStatus.${docKey}`] = "verified";
    await updateDoc(doc(db,"users",uid), updateObj);
    toast(`✅ ${docKey} verified!`);
    App.adminTab("kyc");
  },

  adminLoadDrivers: async function(content) {
    content.innerHTML = '<div style="text-align:center;padding:20px"><div class="search-ring" style="width:36px;height:36px;border-width:3px"></div></div>';
    const [snap, feesSnap, wSnap] = await Promise.all([
      getDocs(query(collection(db,"users"), where("role","==","driver"))),
      getDocs(query(collection(db,"platformFees"), where("paid","==",false))),
      getDocs(query(collection(db,"withdrawals"), where("status","==","pending")))
    ]);
    const pendingFees = {}, pendingWd = {};
    feesSnap.forEach(d => { const f=d.data(); pendingFees[f.driverUid]=(pendingFees[f.driverUid]||0)+f.amount; });
    wSnap.forEach(d => { const w=d.data(); pendingWd[w.driverUid]=(pendingWd[w.driverUid]||0)+w.amount; });
    let html = `<div class="section-label">All Drivers (${snap.size})</div>`;
    snap.forEach(d => {
      const u = d.data();
      const bal = (u.earnings||0)-(u.withdrawn||0);
      const pFee = pendingFees[d.id]||0;
      const pWd = pendingWd[d.id]||0;
      const joinDate = u.joinedAt?new Date(u.joinedAt).toLocaleDateString("en-IN"):"—";
      html += `<div class="card" style="border-left:4px solid ${u.isOnDuty?"var(--green)":u.approvalStatus==="rejected"?"var(--red)":"var(--border)"}">
        <div style="display:flex;justify-content:space-between;align-items:flex-start">
          <div><h4 style="font-size:15px;font-weight:800">${u.name}</h4>
          <p style="font-size:12px;color:var(--muted)">${u.vehicle}·${u.vehicleNum||""}·${u.email||""}</p></div>
          <span class="badge badge-${u.isOnDuty?"online":u.approvalStatus||"trial"}">${u.isOnDuty?"🟢 Online":"⚫ "+(u.approvalStatus||"trial")}</span>
        </div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:6px;margin:10px 0;background:#f8f8f8;border-radius:12px;padding:10px;font-size:12px">
          <div><div style="color:var(--muted);font-weight:700;font-size:10px">PHONE</div><b>${u.phone||"—"}</b></div>
          <div><div style="color:var(--muted);font-weight:700;font-size:10px">JOINED</div><b>${joinDate}</b></div>
          <div><div style="color:var(--muted);font-weight:700;font-size:10px">TOTAL EARNED</div><b style="color:var(--green-dark)">₹${u.earnings||0}</b></div>
          <div><div style="color:var(--muted);font-weight:700;font-size:10px">WITHDRAWN</div><b>₹${u.withdrawn||0}</b></div>
          <div><div style="color:var(--muted);font-weight:700;font-size:10px">WALLET BAL</div><b style="color:var(--blue)">₹${bal}</b></div>
          <div><div style="color:var(--muted);font-weight:700;font-size:10px">RIDES·RATING</div><b>${u.totalRides||0}·⭐${u.avgRating||"New"}</b></div>
          <div><div style="color:var(--muted);font-weight:700;font-size:10px">UPI</div><b style="font-size:11px">${u.upiId||"Not set"}</b></div>
          <div><div style="color:var(--muted);font-weight:700;font-size:10px">STATUS</div><span class="badge badge-${u.approvalStatus||"trial"}" style="font-size:10px">${u.approvalStatus||"trial"}</span></div>
        </div>
        ${pFee>0||pWd>0?`<div style="background:linear-gradient(135deg,#fff3e0,#ffe0b2);border:1.5px solid #ff9800;border-radius:10px;padding:10px 12px;margin-bottom:8px">
          <b style="color:#e65100;font-size:13px">💸 Pending Amounts</b>
          ${pFee>0?`<div style="display:flex;justify-content:space-between;margin-top:6px"><span style="font-size:13px">Platform fee owed:</span><b style="color:var(--red)">₹${pFee}</b></div>`:""}
          ${pWd>0?`<div style="display:flex;justify-content:space-between;margin-top:4px"><span style="font-size:13px">Withdrawal pending:</span><b style="color:var(--blue)">₹${pWd}</b></div>`:""}
        </div>`:""}
        <div style="display:flex;gap:6px;flex-wrap:wrap">
          <button class="btn btn-green btn-sm" onclick="App.adminApproveDriver('${d.id}')">✅ Approve</button>
          <button class="btn btn-red btn-sm" onclick="App.adminSuspendDriver('${d.id}')">🚫 Suspend</button>
          <button class="btn btn-sm btn-gray" onclick="window.open('tel:${u.phone}')">📞 Call</button>
          <a href="https://wa.me/91${u.phone}" class="btn btn-sm btn-gray" target="_blank">📱 WA</a>
          ${pFee>0?`<button class="btn btn-primary btn-sm" onclick="window.open('https://wa.me/91${u.phone}?text=Hi ${u.name}, platform fee of ₹${pFee} is pending. Pay to ${ADMIN_UPI}.')">💸 Remind</button>`:""}
        </div>
      </div>`;
    });
    content.innerHTML = snap.empty ? "<div style='text-align:center;padding:30px;color:var(--muted)'>No drivers yet</div>" : html;
  },

    adminSuspendDriver: async function(uid) {
    if (!confirm("Suspend this driver?")) return;
    await updateDoc(doc(db,"users",uid), { approvalStatus:"rejected", isOnDuty: false });
    toast("Driver suspended.");
    App.adminTab("drivers");
  },

  adminLoadWithdrawals: async function(content) {
    const q = query(collection(db,"withdrawals"), orderBy("requestedAt","desc"), limit(50));
    const snap = await getDocs(q);
    let html = `<div class="card" style="text-align:center;background:linear-gradient(135deg,#e8f5e9,#c8e6c9);margin:10px">
      <p style="font-weight:800;color:var(--green-dark)">💳 Pay to drivers via their UPI ID shown below</p>
    </div><div class="section-label">Withdrawal Requests</div>`;
    snap.forEach(d => {
      const w = d.data();
      html += `<div class="card" style="border-left:4px solid ${w.status==="pending"?"var(--yellow)":w.status==="paid"?"var(--green)":"var(--red)"}">
        <div style="display:flex;justify-content:space-between">
          <h4 style="font-size:16px;font-weight:800">₹${w.amount}</h4>
          <span class="badge badge-${w.status==="paid"?"approved":w.status==="rejected"?"rejected":"pending"}">${w.status}</span>
        </div>
        <p style="margin:4px 0"><b>${w.driverName}</b> · ${w.driverPhone}</p>
        <p style="font-size:13px;color:var(--muted)">UPI: ${w.upiId}</p>
        ${w.note?`<p style="font-size:12px;color:var(--muted)">Note: ${w.note}</p>`:""}
        <p style="font-size:12px;color:var(--muted)">${new Date(w.requestedAt).toLocaleString("en-IN")}</p>
        ${w.status==="pending"?`<div style="display:flex;gap:8px;margin-top:10px;flex-wrap:wrap">
          <button class="btn btn-green btn-sm" onclick="App.adminPayWithdraw('${d.id}','${w.driverUid}','${w.amount}')">💸 Mark Paid</button>
          <button class="btn btn-red btn-sm" onclick="App.adminRejectWithdraw('${d.id}')">❌ Reject</button>
          <a href="https://wa.me/91${w.driverPhone}?text=Hi ${w.driverName}, your withdrawal of Rs.${w.amount} has been processed." class="btn btn-gray btn-sm" target="_blank">📱 Notify</a>
        </div>`:""}
      </div>`;
    });
    content.innerHTML = snap.empty ? "<div style='text-align:center;padding:30px;color:var(--muted)'>No withdrawal requests</div>" : html;
  },

  adminPayWithdraw: async function(wId, driverUid, amount) {
    if (!confirm(`Mark ₹${amount} as paid?`)) return;
    await updateDoc(doc(db,"withdrawals",wId), { status:"paid", paidAt: new Date().toISOString() });
    if (driverUid) await updateDoc(doc(db,"users",driverUid), { withdrawn: increment(parseInt(amount)) });
    toast("✅ Marked as paid!");
    App.adminTab("withdrawals");
  },

  adminRejectWithdraw: async function(wId) {
    const reason = prompt("Rejection reason:");
    if (!reason) return;
    await updateDoc(doc(db,"withdrawals",wId), { status:"rejected", rejectionReason: reason });
    toast("Withdrawal rejected.");
    App.adminTab("withdrawals");
  },

  // Admin platform fees management
  adminLoadPlatformFees: async function(content) {
    const q = query(collection(db,"platformFees"), orderBy("createdAt","desc"), limit(50));
    const snap = await getDocs(q);
    let totalPending = 0, totalCollected = 0;
    snap.docs.forEach(d => {
      const f = d.data();
      if (!f.paid) totalPending += f.amount;
      else totalCollected += f.amount;
    });
    let html = `
      <div class="earnings-grid">
        <div class="earnings-item"><div class="amount" style="color:var(--red)">₹${totalPending}</div><div class="label">Pending Fees</div></div>
        <div class="earnings-item"><div class="amount">₹${totalCollected}</div><div class="label">Collected Fees</div></div>
      </div>
      <div class="section-label">Platform Fee Records</div>`;
    const now = new Date();
    snap.forEach(d => {
      const f = d.data();
      const overdue = !f.paid && now > new Date(f.dueBy);
      html += `<div class="card" style="border-left:4px solid ${f.paid?"var(--green)":overdue?"var(--red)":"var(--yellow)"}">
        <div style="display:flex;justify-content:space-between">
          <h4>₹${f.amount} — ${f.driverName}</h4>
          <span class="badge badge-${f.paid?"approved":overdue?"rejected":"pending"}">${f.paid?"PAID":overdue?"OVERDUE":"PENDING"}</span>
        </div>
        <p>Driver Phone: ${f.driverPhone}</p>
        <p style="font-size:13px;color:var(--muted)">Due: ${new Date(f.dueBy).toLocaleString("en-IN")}</p>
        <p style="font-size:12px;color:var(--muted)">${new Date(f.createdAt).toLocaleString("en-IN")}</p>
        ${!f.paid?`<div style="display:flex;gap:8px;margin-top:8px;flex-wrap:wrap">
          <button class="btn btn-green btn-sm" onclick="App.adminConfirmFeePaid('${d.id}','${f.driverUid}')">✅ Confirm Received</button>
          ${overdue?`<button class="btn btn-red btn-sm" onclick="App.adminSuspendDriver('${f.driverUid}')">🚫 Suspend Driver</button>`:""}
          <a href="https://wa.me/91${f.driverPhone}?text=Hi ${f.driverName}, your platform fee of Rs.${f.amount} is ${overdue?"OVERDUE":"due by "+new Date(f.dueBy).toLocaleDateString()}. Please pay to ${ADMIN_UPI} immediately." class="btn btn-gray btn-sm" target="_blank">📱 Remind</a>
        </div>`:""}
      </div>`;
    });
    content.innerHTML = snap.empty ? "<div style='text-align:center;padding:30px;color:var(--muted)'>No fee records</div>" : html;
  },

  adminConfirmFeePaid: async function(feeId, driverUid) {
    if (!confirm("Confirm platform fee received?")) return;
    await updateDoc(doc(db,"platformFees",feeId), { paid: true, paidAt: new Date().toISOString(), confirmedByAdmin: true });
    toast("✅ Fee marked as received!");
    App.adminTab("fees");
  },

  adminLoadAnalytics: async function(content) {
    const [ridesSnap, driversSnap, feesSnap] = await Promise.all([
      getDocs(collection(db,"rides")),
      getDocs(query(collection(db,"users"),where("role","==","driver"))),
      getDocs(collection(db,"platformFees"))
    ]);
    const rides = ridesSnap.docs.map(d=>d.data());
    const completed = rides.filter(r=>r.status==="completed");
    const cancelled = rides.filter(r=>r.status==="cancelled");
    const pending   = rides.filter(r=>r.status==="pending");
    const totalRevenue = completed.reduce((a,r)=>a+(r.fare||0),0);
    const platformCut = Math.round(totalRevenue*0.20);
    const feeCollected = feesSnap.docs.filter(d=>d.data().paid).reduce((a,d)=>a+(d.data().amount||0),0);
    const bikeRides = completed.filter(r=>r.vehicle==="Bike").length;
    const autoRides = completed.filter(r=>r.vehicle==="Auto").length;
    const carRides  = completed.filter(r=>r.vehicle==="Car").length;
    const approved  = driversSnap.docs.filter(d=>d.data().approvalStatus==="approved").length;
    const trial     = driversSnap.docs.filter(d=>d.data().approvalStatus==="trial").length;
    const online    = driversSnap.docs.filter(d=>d.data().isOnDuty).length;
    content.innerHTML = `<div class="admin-stat-grid">
      <div class="stat-card"><div class="stat-num">${completed.length}</div><div class="stat-label">Completed Rides</div></div>
      <div class="stat-card"><div class="stat-num">${cancelled.length}</div><div class="stat-label">Cancelled Rides</div></div>
      <div class="stat-card"><div class="stat-num">₹${totalRevenue}</div><div class="stat-label">Total Revenue</div></div>
      <div class="stat-card"><div class="stat-num">₹${platformCut}</div><div class="stat-label">Platform Share (20%)</div></div>
      <div class="stat-card"><div class="stat-num" style="color:var(--green-dark)">₹${feeCollected}</div><div class="stat-label">Fees Collected</div></div>
      <div class="stat-card"><div class="stat-num">${driversSnap.size}</div><div class="stat-label">Total Drivers</div></div>
      <div class="stat-card"><div class="stat-num">${online}</div><div class="stat-label">Online Now</div></div>
      <div class="stat-card"><div class="stat-num">${approved}</div><div class="stat-label">Verified Drivers</div></div>
      <div class="stat-card"><div class="stat-num">${trial}</div><div class="stat-label">Trial Drivers</div></div>
      <div class="stat-card"><div class="stat-num">${pending.length}</div><div class="stat-label">Pending Rides</div></div>
      <div class="stat-card"><div class="stat-num">${bikeRides}</div><div class="stat-label">Bike Rides</div></div>
      <div class="stat-card"><div class="stat-num">${autoRides}</div><div class="stat-label">Auto Rides</div></div>
      <div class="stat-card"><div class="stat-num">${carRides}</div><div class="stat-label">Car Rides</div></div>
    </div>`;
  },

  adminLogout: async function() {
    if (unsubAdmin) { unsubAdmin(); unsubAdmin = null; }
    await signOut(auth);
    state.currentUser = null; state.role = null;
    showScreen("screen-landing");
    toast("👋 Logged out");
  },


  _modal(html){document.body.insertAdjacentHTML("beforeend",`<div class="ai-modal-base" onclick="if(event.target===this)this.remove()"><div class="ai-modal-inner"><div class="ai-modal-handle"></div>${html}<button style="width:100%;background:#f0f0f0;border:none;border-radius:12px;padding:14px;font-weight:800;font-family:'Nunito',sans-serif;font-size:14px;cursor:pointer;margin-top:8px" onclick="this.closest('.ai-modal-base').remove()">Close</button></div></div>`);},

  aiSuggest:function(){
    const h=new Date().getHours(),time=h<12?"Morning":h<17?"Afternoon":"Evening";
    const s=h<10?["🏢 Office/
