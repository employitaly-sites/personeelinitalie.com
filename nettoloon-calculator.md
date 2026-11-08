# Nettoloon-calculator Italië

> Van bruto naar netto in Italië in 2026: werknemerspremies, inkomstenbelasting (IRPEF) met arbeidskortingen, regionale en gemeentelijke opslagen, voor Milaan, Rome, Turijn, Bologna en Napels.


<section> <span class="eyebrow">Nettoloon-calculator</span> <h1 id="wat-uw-werknemer-in-italie-overhoudt">Wat uw werknemer in Italië overhoudt</h1> <p class="lede">Vul het bruto jaarsalaris en de stad in. De schatting past de regels van 2026 toe: werknemerspremies, inkomstenbelasting met arbeidskortingen en de lokale opslagen.</p> </section> <section> <div class="grid"> <form class="panel" id="nf" style="display:grid;gap:16px"> <label for="g">Bruto jaarsalaris, RAL (EUR)<input id="g" type="number" min="10000" step="1000" value="50000"></label> <label for="city">Stad<select id="city"><option value="Milan">Milaan</option><option value="Rome">Rome</option><option value="Turin">Turijn</option><option value="Bologna">Bologna</option><option value="Naples">Napels</option></select></label> <label for="inst">Betaald in<select id="inst"><option value="13">13 termijnen</option><option value="14">14 termijnen</option><option value="12">12 (13e/14e naar rato)</option></select></label> </form> <div class="panel" style="min-width:0"><div class="tbl"><table id="nt"><thead><tr><th>Post</th><th class="num">Per jaar</th></tr></thead><tbody></tbody></table></div> <p id="nm" style="margin-bottom:0;font-weight:600"></p></div> </div>  </section> <section class="prose"> <h2 id="zo-werkt-de-berekening">Zo werkt de berekening</h2> <ol> <li>De werknemer betaalt 9,19% sociale premie over het brutoloon, plus 1% over het deel boven €56.224 per jaar.</li> <li>Over de rest geldt inkomstenbelasting (IRPEF): 23% tot €28.000, 33% van €28.000 tot €50.000, 43% daarboven.</li> <li>Arbeidskortingen verlagen de belasting. Ze zijn het hoogst bij lage salarissen en worden nul bij €50.000. Tussen €20.000 en €40.000 geldt een extra korting tot €1.000.</li> <li>De regionale en gemeentelijke opslagen komen erbij, tenzij het inkomen onder de vrijstellingsgrens van de stad ligt.</li> </ol> <p>Een loonheffingskorting zoals in Nederland kiest de werknemer hier niet zelf: de kortingen worden automatisch in de verloning toegepast. Niets hiervan verandert wat u als werkgever betaalt: zie de <a href="kostencalculator.html">loonkostencalculator</a>.</p> </section> <script>(function(){const N = {"status":"secondary","source":"data/params/2026.yaml (irpef, inps, addizionali); TUIR art. 11 and 13; L. 207/2024; L. 199/2025","employee_inps":0.0919,"extra_1pct_above":56224,"irpef":[[28000,0.23],[50000,0.33],[null,0.43]],"cities":{"Milan":{"region":"Lombardije","regional":[[null,0.0123]],"municipal":0.008,"exempt":23000},"Rome":{"region":"Lazio","regional":[[28000,0.0173],[null,0.0333]],"municipal":0.009,"exempt":14000},"Turin":{"region":"Piëmont","regional":[[null,0.0162]],"municipal":0.008,"municipal_note":"0,8% tot 1,2% per schijf; 0,8% gebruikt","exempt":11790},"Bologna":{"region":"Emilia-Romagna","regional":[[null,0.0133]],"municipal":0.008,"exempt":15000},"Naples":{"region":"Campanië","regional":[[null,0.0173]],"municipal":0.01,"exempt":12000}}};

const eur = n => (n < 0 ? '−€' : '€') + Math.round(Math.abs(n)).toLocaleString('nl-NL');
function bands(x, b){ let t = 0, lo = 0; for (const [hi, r] of b){ const top = hi ?? Infinity; if (x > lo) t += (Math.min(x, top) - lo) * r; lo = top; } return t; }
function credit(r){ if (r <= 15000) return 1955; if (r <= 28000) return 1910 + 1190 * (28000 - r) / 13000; if (r <= 50000) return 1910 * (50000 - r) / 22000 + (r > 25000 && r <= 35000 ? 65 : 0); return 0; }
function extra(r){ if (r > 20000 && r <= 32000) return 1000; if (r > 32000 && r < 40000) return 1000 * (40000 - r) / 8000; return 0; }
function calc(){
  const g = +document.getElementById('g').value || 0, c = N.cities[document.getElementById('city').value], n = +document.getElementById('inst').value;
  const inps = g * N.employee_inps + Math.max(0, g - N.extra_1pct_above) * 0.01;
  const tx = g - inps;
  const irpef = Math.max(0, bands(tx, N.irpef) - credit(tx) - extra(tx));
  const reg = bands(tx, c.regional), mun = tx > c.exempt ? tx * c.municipal : 0;
  const net = g - inps - irpef - reg - mun;
  const rows = [['Brutosalaris', g], ['Werknemerspremies', -inps], ['Inkomstenbelasting (IRPEF) na kortingen', -irpef], [`Regionale opslag (${c.region})`, -reg], ['Gemeentelijke opslag', -mun]];
  document.querySelector('#nt tbody').innerHTML = rows.map(([k, v]) => `<tr><td>${k}</td><td class="num">${eur(v)}</td></tr>`).join('') + `<tr><td><b>Nettoloon</b></td><td class="num"><b>${eur(net)}</b></td></tr>`;
  document.getElementById('nm').textContent = `Ongeveer ${eur(net / n)} netto in elk van ${n} maandstroken (${(net / g * 100).toFixed(0)}% van bruto).`;
}
document.getElementById('nf').addEventListener('input', calc); document.getElementById('nf').addEventListener('submit', e => e.preventDefault()); calc();
})();</script>


---
Source: https://personeelinitalie.com/nettoloon-calculator.html
