[script.js](https://github.com/user-attachments/files/34854749/script.js)
/* ===== GALERÍA DE PORTADA ===== */
const slides = document.querySelectorAll('.slide');
const puntos = document.querySelector('.puntos');
let actual = 0, timer;

slides.forEach((_, i) => {
    const b = document.createElement('button');
    b.setAttribute('aria-label', 'Imagen ' + (i + 1));
    b.addEventListener('click', () => { mostrar(i); reiniciar(); });
    puntos.appendChild(b);
});

function mostrar(n) {
    actual = (n + slides.length) % slides.length;
    slides.forEach((s, i) => s.classList.toggle('on', i === actual));
    puntos.querySelectorAll('button').forEach((b, i) => b.classList.toggle('on', i === actual));
}
function reiniciar() { clearInterval(timer); timer = setInterval(() => mostrar(actual + 1), 5000); }

document.querySelector('.flecha.izq').addEventListener('click', () => { mostrar(actual - 1); reiniciar(); });
document.querySelector('.flecha.der').addEventListener('click', () => { mostrar(actual + 1); reiniciar(); });
mostrar(0); reiniciar();

/* ===== LISTAS DESPLEGABLES ===== */
document.querySelectorAll('.acc').forEach(btn => {
    btn.addEventListener('click', () => btn.parentElement.classList.toggle('open'));
});

/* ===== PESTAÑAS ===== */
document.querySelectorAll('.tab').forEach(tab => {
    tab.addEventListener('click', () => {
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('on'));
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('on'));
        tab.classList.add('on');
        document.getElementById(tab.dataset.panel).classList.add('on');
    });
});

/* ===== APARECER AL HACER SCROLL + MENÚ ACTIVO ===== */
const obs = new IntersectionObserver(entradas => {
    entradas.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); obs.unobserve(e.target); } });
}, { threshold: 0.15 });
document.querySelectorAll('.reveal').forEach(el => obs.observe(el));

const enlaces = document.querySelectorAll('nav a');
const secciones = [...enlaces].map(a => document.querySelector(a.getAttribute('href')));
window.addEventListener('scroll', () => {
    let idx = 0;
    secciones.forEach((s, i) => { if (s && s.getBoundingClientRect().top < 140) idx = i; });
    enlaces.forEach((a, i) => a.classList.toggle('activo', i === idx));
});

/* ===== CALCULADORA DE ALIMENTO ===== */
document.getElementById('formCalc').addEventListener('submit', e => {
    e.preventDefault();
    const especie = document.getElementById('especie').value;
    const peso = parseFloat(document.getElementById('peso').value);
    const edad = document.getElementById('edad').value;
    const act = document.getElementById('actividad').value;
    const caja = document.getElementById('resultado');
    if (!(peso > 0)) return;

    // Energía en reposo (kcal) = 70 x peso^0.75, multiplicada por un factor
    const factor = {
        perro: { cachorro: 2.0, adulto: 1.6, senior: 1.2 },
        gato:  { cachorro: 2.5, adulto: 1.2, senior: 1.1 }
    }[especie][edad];
    const ajuste = { baja: 0.85, normal: 1, alta: 1.2 }[act];

    const kcal = 70 * Math.pow(peso, 0.75) * factor * ajuste;
    const gramos = Math.round(kcal / 3.6);           // croquetas ≈ 3.6 kcal por gramo
    const comidas = edad === 'cachorro' ? 4 : (especie === 'gato' ? 3 : 2);
    const porComida = Math.round(gramos / comidas);

    caja.innerHTML = `<div><div class="res-caja">
        <div class="grande"><span id="contador">0</span> g al día</div>
        <p>Repartidos en <strong>${comidas} comidas</strong> de unos <strong>${porComida} g</strong> cada una.</p>
        <small>Aprox. ${Math.round(kcal)} kcal diarias. Es una estimación: ajusta según el empaque del alimento y las indicaciones de tu veterinario.</small>
    </div></div>`;
    caja.classList.add('on');

    // Número que sube hasta el resultado
    const cont = document.getElementById('contador');
    let n = 0;
    const paso = Math.max(1, Math.round(gramos / 40));
    const t = setInterval(() => {
        n = Math.min(gramos, n + paso);
        cont.textContent = n;
        if (n >= gramos) clearInterval(t);
    }, 25);
});

/* ===== CONSEJOS ===== */
const consejos = [
    'Cambia el alimento poco a poco, mezclándolo durante 7 días.',
    'Pesa a tu mascota cada mes para ajustar la porción.',
    'Los premios no deben pasar del 10% de la comida diaria.',
    'Lava el plato de agua y comida todos los días.',
    'Los gatos prefieren varias porciones pequeñas a una grande.',
    'Un paseo corto después de comer ayuda a la digestión del perro.'
];
const pCons = document.getElementById('consejo');
let ci = 0;
pCons.textContent = consejos[ci];
document.getElementById('otroConsejo').addEventListener('click', () => {
    pCons.classList.add('cambia');
    setTimeout(() => { ci = (ci + 1) % consejos.length; pCons.textContent = consejos[ci]; pCons.classList.remove('cambia'); }, 300);
});
