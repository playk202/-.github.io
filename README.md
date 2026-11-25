<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Кредитный калькулятор: платёж и переплата онлайн</title>
<meta name="description" content="Рассчитайте ежемесячный платёж, переплату и график платежей по кредиту. Аннуитетные и дифференцированные платежи.">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;800&display=swap">
<style>
:root{--bg:#eef2f0;--card:#fff;--ink:#14231f;--mute:#5a6b66;--line:#d5dedb;--acc:#0f6b5c;--acc-ink:#fff;--warn:#c2410c;
box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0e1815;--card:#16241f;--ink:#e6f0ec;--mute:#92a69f;--line:#2a3d37;--acc:#3fc2a8;--acc-ink:#06201a;--warn:#fb923c}}
:root[data-theme="dark"]{--bg:#0e1815;--card:#16241f;--ink:#e6f0ec;--mute:#92a69f;--line:#2a3d37;--acc:#3fc2a8;--acc-ink:#06201a;--warn:#fb923c}
*{box-sizing:border-box}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.55 Manrope,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif}
main{max-width:720px;margin:0 auto;padding:20px 16px 48px}
h1{font-size:clamp(26px,6vw,36px);line-height:1.15;font-weight:800;margin:8px 0 6px}
h2{font-size:20px;margin:32px 0 8px}
.lead{color:var(--mute);margin:0 0 20px}
.card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:18px}
.row{display:grid;gap:14px;grid-template-columns:1fr 1fr}
.full{grid-column:1/-1}
label{display:block;font-weight:600;font-size:14px;margin-bottom:4px}
input,select{width:100%;font:inherit;color:var(--ink);background:var(--bg);border:1px solid var(--line);border-radius:8px;padding:10px 12px;min-height:44px}
input:focus-visible,select:focus-visible,button:focus-visible{outline:3px solid var(--acc);outline-offset:1px}
.seg{display:flex;gap:6px}
.seg button{flex:1;font:inherit;font-weight:600;min-height:44px;border-radius:8px;border:1px solid var(--line);background:var(--bg);color:var(--ink);cursor:pointer}
.seg button[aria-pressed="true"]{background:var(--acc);color:var(--acc-ink);border-color:var(--acc)}
.result{margin-top:16px}
.big{font-size:clamp(34px,10vw,52px);font-weight:800;line-height:1.05;color:var(--acc);margin:2px 0}
.small{color:var(--mute);font-size:14px}
.bar{display:flex;height:14px;border-radius:7px;overflow:hidden;margin:14px 0 8px;background:var(--line)}
.bar i{display:block;height:100%}
.legend{display:flex;justify-content:space-between;gap:10px;flex-wrap:wrap;font-size:14px}
.dot{display:inline-block;width:10px;height:10px;border-radius:50%;margin-right:6px}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:14px}
.stats b{display:block;font-size:19px}
.ad{margin:24px 0;min-height:90px;border:1px dashed var(--line);border-radius:10px;display:flex;align-items:center;justify-content:center;color:var(--mute);font-size:13px}
.scroll{overflow-x:auto;margin-top:10px}
table{border-collapse:collapse;width:100%;min-width:480px;font-size:14px}
th,td{padding:8px 10px;text-align:right;border-bottom:1px solid var(--line)}
th:first-child,td:first-child{text-align:left}
th{color:var(--mute);font-weight:600}
details{border-bottom:1px solid var(--line);padding:10px 0}
summary{font-weight:600;cursor:pointer}
details p{margin:8px 0 0;color:var(--mute)}
footer{margin-top:32px;color:var(--mute);font-size:13px}
@media (max-width:420px){.row{grid-template-columns:1fr}}
</style>
<script>window.yaContextCb=window.yaContextCb||[]</script>
<script src="https://yandex.ru/ads/system/context.js" async></script>
</head>
<body>
<main>
<h1>Кредитный калькулятор</h1>
<p class="lead">Узнайте ежемесячный платёж, переплату и график платежей за несколько секунд.</p>

<section class="card" aria-label="Калькулятор">
<div class="row">
<div class="full"><label for="sum">Сумма кредита, ₽</label><input id="sum" type="number" inputmode="decimal" min="1" value="1500000"></div>
<div><label for="rate">Ставка, % годовых</label><input id="rate" type="number" inputmode="decimal" min="0" step="0.1" value="16"></div>
<div><label for="term">Срок</label><input id="term" type="number" inputmode="numeric" min="1" value="5"></div>
<div><label>Срок в</label><div class="seg" id="unit"><button type="button" data-v="y" aria-pressed="true">годах</button><button type="button" data-v="m" aria-pressed="false">месяцах</button></div></div>
<div><label>Тип платежей</label><div class="seg" id="kind"><button type="button" data-v="a" aria-pressed="true">Аннуитет</button><button type="button" data-v="d" aria-pressed="false">Дифф.</button></div></div>
</div>
<div class="result" aria-live="polite">
<div class="small" id="plabel">Ежемесячный платёж</div>
<div class="big" id="pay">—</div>
<div class="bar" aria-hidden="true"><i id="b1" style="background:var(--acc)"></i><i id="b2" style="background:var(--warn)"></i></div>
<div class="legend"><span><span class="dot" style="background:var(--acc)"></span>Основной долг <b id="lp">—</b></span><span><span class="dot" style="background:var(--warn)"></span>Проценты <b id="li">—</b></span></div>
<div class="stats"><div class="small">Переплата<b id="over">—</b></div><div class="small">Всего выплатите<b id="tot">—</b></div></div>
</div>
</section>

<!-- Замените R-A-XXXXXXX-1 на ID вашего блока из кабинета РСЯ -->
<div class="ad" id="ad1"><div id="yandex_rtb_R-A-XXXXXXX-1"></div></div>
<script>window.yaContextCb.push(function(){Ya.Context.AdvManager.render({blockId:"R-A-XXXXXXX-1",renderTo:"yandex_rtb_R-A-XXXXXXX-1"})})</script>

<h2>График платежей (первый год)</h2>
<div class="scroll"><table><thead><tr><th>Месяц</th><th>Платёж</th><th>Долг</th><th>Проценты</th><th>Остаток</th></tr></thead><tbody id="sch"></tbody></table></div>

<!-- Замените R-A-XXXXXXX-2 на ID второго блока -->
<div class="ad" id="ad2"><div id="yandex_rtb_R-A-XXXXXXX-2"></div></div>
<script>window.yaContextCb.push(function(){Ya.Context.AdvManager.render({blockId:"R-A-XXXXXXX-2",renderTo:"yandex_rtb_R-A-XXXXXXX-2"})})</script>

<h2>Как считается кредит</h2>
<p class="small" style="font-size:16px">При аннуитетной схеме платёж одинаков каждый месяц: в начале срока большая часть уходит на проценты, в конце — на основной долг. При дифференцированной схеме основной долг гасится равными частями, поэтому платёж постепенно уменьшается, а переплата меньше.</p>

<h2>Частые вопросы</h2>
<details><summary>Что выгоднее: аннуитетный или дифференцированный платёж?</summary><p>Дифференцированный даёт меньшую переплату, но первые платежи выше. Аннуитетный проще планировать, зато проценты за весь срок больше.</p></details>
<details><summary>Как уменьшить переплату?</summary><p>Выбрать меньший срок, увеличить первоначальный взнос или вносить досрочные платежи. Чем раньше вы гасите долг, тем меньше начисляется процентов.</p></details>
<details><summary>Точен ли расчёт?</summary><p>Расчёт ориентировочный: банк может добавить страховку и комиссии, а день платежа влияет на проценты. Точные цифры смотрите в договоре.</p></details>

<footer>Калькулятор носит информационный характер и не является предложением кредита.</footer>
</main>
<script>
(function(){
var $=function(i){return document.getElementById(i)};
var unit='y',kind='a';
var nf=new Intl.NumberFormat('ru-RU',{maximumFractionDigits:0});
function f(x){return nf.format(Math.round(x))+' ₽'}
function seg(id,cb){var box=$(id);box.addEventListener('click',function(e){var b=e.target.closest('button');if(!b)return;
[].forEach.call(box.children,function(c){c.setAttribute('aria-pressed',c===b?'true':'false')});cb(b.dataset.v);calc()})}
seg('unit',function(v){unit=v});
seg('kind',function(v){kind=v});
function calc(){
var S=parseFloat($('sum').value),R=parseFloat($('rate').value),T=parseFloat($('term').value);
var n=Math.round(unit==='y'?T*12:T);
if(!(S>0)||!(R>=0)||!(n>0)||n>600){$('pay').textContent='—';$('sch').innerHTML='';return}
var r=R/100/12,rows=[],bal=S,sumInt=0,first=0,last=0,p,i,int,pr;
if(kind==='a'){p=r===0?S/n:S*r/(1-Math.pow(1+r,-n))}
for(i=1;i<=n;i++){
int=bal*r;
if(kind==='a'){pr=p-int}else{pr=S/n;p=pr+int}
if(i===1)first=p;if(i===n)last=p;
bal=Math.max(0,bal-pr);sumInt+=int;
if(i<=12)rows.push('<tr><td>'+i+'</td><td>'+nf.format(p)+'</td><td>'+nf.format(pr)+'</td><td>'+nf.format(int)+'</td><td>'+nf.format(bal)+'</td></tr>')}
var total=S+sumInt;
if(kind==='a'){$('plabel').textContent='Ежемесячный платёж';$('pay').textContent=f(first)}
else{$('plabel').textContent='Платёж: от последнего до первого';$('pay').textContent=f(last)+' – '+f(first)}
$('lp').textContent=f(S);$('li').textContent=f(sumInt);
$('over').textContent=f(sumInt);$('tot').textContent=f(total);
$('b1').style.width=(S/total*100)+'%';$('b2').style.width=(sumInt/total*100)+'%';
$('sch').innerHTML=rows.join('')}
['sum','rate','term'].forEach(function(id){$(id).addEventListener('input',calc)});
calc();
})();
</script>
</body>
</html>
