# Offerte aanvragen

> Vraag een offerte aan om een werknemer in Italië in dienst te nemen. Drie korte stappen, antwoord binnen één werkdag.


<section> <span class="eyebrow">Offerte aanvragen</span> <h1 id="vertel-ons-wie-u-wilt-aannemen">Vertel ons wie u wilt aannemen</h1> <p class="lede">Drie korte stappen. Binnen één werkdag krijgt u de volledige kosten en de opzet van het contract.</p> </section> <section> <form id="lead" class="panel" style="display:grid;gap:18px;max-width:640px" novalidate> <p id="stepinfo" class="tag" style="justify-self:start">Stap 1 van 3</p> <fieldset data-step="1" style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="email">Zakelijk e-mailadres<input id="email" type="email" autocomplete="email" required></label> <label for="company">Bedrijf<input id="company" autocomplete="organization"></label> </fieldset> <fieldset data-step="2" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="service">Wat hebt u nodig?<select id="service"><option>Employer of Record</option><option>Salarisadministratie (we hebben een Italiaanse vestiging)</option><option>Werkvergunning / Blue Card</option><option>Werving</option></select></label> <label for="count">Hoeveel mensen in Italië?<input id="count" type="number" min="1" value="1"></label> <label for="city">Stad of regio<input id="city" placeholder="Milaan, Rome, op afstand…"></label> <label for="salary">Beoogd bruto jaarsalaris (EUR)<input id="salary" type="number" step="1000"></label> </fieldset> <fieldset data-step="3" hidden style="border:0;padding:0;margin:0;display:grid;gap:14px"> <label for="name">Uw naam<input id="name" autocomplete="name"></label> <label for="start">Verwachte startdatum<input id="start" type="date"></label> <label for="notes">Overige opmerkingen<textarea id="notes" rows="4"></textarea></label> <label style="display:flex;gap:10px;align-items:flex-start;font-weight:400"><input id="consent" type="checkbox" style="width:auto;margin-top:5px">Ik ga ermee akkoord dat PersoneelInItalië mijn gegevens verwerkt om deze aanvraag te beantwoorden (privacybeleid).</label> </fieldset> <p id="msg" role="status" style="margin:0;color:var(--accent)"></p> <div class="cta"><button type="button" class="btn ghost" id="back" hidden>Terug</button><button type="submit" class="btn" id="next">Verder</button></div> </form> </section> <script>
// Il passo 1 salva subito l'email (lead abbandonato recuperabile dal nurturing). Endpoint del worker: /api/lead.
const f = document.getElementById('lead'); let step = 1;
// Oggetto dalla pagina partner: ?subject=Partner precompila le note.
{ const sj = new URLSearchParams(location.search).get('subject'); if (sj) document.getElementById('notes').value = '[' + sj.slice(0, 40) + '] '; }
const sets = [...f.querySelectorAll('fieldset')];
const data = () => Object.fromEntries([...f.querySelectorAll('input,select,textarea')].map(e => [e.id, e.type === 'checkbox' ? e.checked : e.value]));
async function save(final){ try { await fetch('/api/lead', {method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({...data(), step, final, brand:'personeelinitalie'})}); } catch(e){} }
function show(){ sets.forEach(s => s.hidden = +s.dataset.step !== step);
  document.getElementById('stepinfo').textContent = `Stap ${step} van 3`;
  document.getElementById('back').hidden = step === 1;
  document.getElementById('next').textContent = step === 3 ? 'Aanvraag versturen' : 'Verder'; }
// Ripresa dal link nelle email: ?resume=<token> precompila i campi e salta al passo successivo.
(async () => { const tok = new URLSearchParams(location.search).get('resume'); if (!tok) return;
  try { const r = await fetch('/api/lead/' + encodeURIComponent(tok)); if (!r.ok) return; const d = await r.json();
    for (const [k, v] of Object.entries(d)) { const el = document.getElementById(k); if (el && v != null) el.value = v; }
    step = d.step || 2; show(); } catch(e){} })();
document.getElementById('back').onclick = () => { step--; show(); };
f.addEventListener('submit', e => { e.preventDefault(); const m = document.getElementById('msg'); m.textContent = '';
  if (step === 1 && !document.getElementById('email').checkValidity()) { m.textContent = 'Vul een geldig zakelijk e-mailadres in om verder te gaan.'; return; }
  if (step === 3 && !document.getElementById('consent').checked) { m.textContent = 'Vink het toestemmingsvakje aan zodat we kunnen antwoorden.'; return; }
  save(step === 3);
  if (step < 3) { step++; show(); } else { f.querySelectorAll('fieldset,.cta').forEach(x => x.hidden = true); m.textContent = 'Dank u. We antwoorden binnen één werkdag.'; } });
</script>


---
Source: https://personeelinitalie.com/offerte.html
