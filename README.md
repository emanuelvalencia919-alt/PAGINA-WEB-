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

/* ===== DATOS: UN ANIMAL POR PESTAÑA ===== */
const animales = [
  { id: 'perros', n: 'Perros', e: '🐶', i: [
    ['Qué darle', ['Croquetas de buena calidad según su edad', 'Pollo o pavo cocido, sin piel ni hueso', 'Arroz, zanahoria y calabaza cocidas', 'Agua limpia siempre disponible']],
    ['Porción y horarios', ['Usa la calculadora de arriba para la cantidad', 'Cachorros: 3 a 4 comidas; adultos: 2 comidas', 'Evita ejercicio intenso justo después de comer']],
    ['Qué evitar', ['Chocolate, uvas y pasas', 'Cebolla, ajo y aguacate', 'Huesos cocidos y alimentos con xilitol']],
    ['Cuidados', ['Paseos diarios y juegos', 'Vacunas y desparasitación al día', 'Cepillado semanal y revisión de oídos y dientes']]
  ]},
  { id: 'gatos', n: 'Gatos', e: '🐱', i: [
    ['Qué darle', ['Alimento para gatos (necesitan taurina)', 'Comida húmeda varias veces por semana', 'Pollo o pescado cocido, sin espinas ni sal', 'Agua fresca; una fuente los anima a beber']],
    ['Porción y horarios', ['Usa la calculadora de arriba para la cantidad', 'Varias porciones pequeñas al día (2 a 3)', 'Comedero lejos del arenero']],
    ['Qué evitar', ['Cebolla, ajo, chocolate y uvas', 'Leche de vaca (muchos no la toleran)', 'Comida de perro como dieta fija']],
    ['Cuidados', ['Rascadores, juguetes y sitios altos', 'Arenero limpio todos los días', 'Vacunas, desparasitación y revisión anual']]
  ]},
  { id: 'conejos', n: 'Conejos', e: '🐰', i: [
    ['Qué darle', ['Heno de buena calidad siempre disponible (es la base)', 'Verduras de hoja verde frescas a diario', 'Pellets para conejo en poca cantidad', 'Agua limpia']],
    ['Porción y horarios', ['Pellets: 1 a 2 cucharadas por kg de peso al día', 'Heno ilimitado', 'Fruta solo como premio, en trozos pequeños']],
    ['Qué evitar', ['Lechuga iceberg, papa, cebolla y ajo', 'Chocolate, pan, galletas y dulces', 'Zanahoria en exceso (tiene mucho azúcar)']],
    ['Cuidados', ['Sus dientes no dejan de crecer: dale heno y madera para roer', 'Espacio amplio para correr cada día', 'No bañarlo en agua; vacunas y revisión veterinaria']]
  ]},
  { id: 'hamsters', n: 'Hámsters', e: '🐹', i: [
    ['Qué darle', ['Mezcla de semillas y pellets para hámster', 'Verduras en trozos pequeños: zanahoria, brócoli, pepino', 'Proteína ocasional: huevo cocido o gusanos de harina', 'Agua fresca en bebedero']],
    ['Porción y horarios', ['1 a 2 cucharaditas de mezcla al día', 'Dar la comida al atardecer, cuando se activan', 'Verdura fresca 1 o 2 veces por semana']],
    ['Qué evitar', ['Cítricos, cebolla, ajo y chocolate', 'Dulces y comida salada', 'Semillas de manzana y almendras amargas']],
    ['Cuidados', ['Rueda del tamaño adecuado y silenciosa', 'Sustrato profundo para que cave', 'Limpieza parcial cada semana; el hámster sirio vive solo']]
  ]},
  { id: 'cobayas', n: 'Cobayas', e: '🐹', i: [
    ['Qué darle', ['Heno siempre disponible', 'Pellets para cobayo (no de conejo)', 'Verduras con vitamina C: pimentón, perejil, cilantro', 'Agua fresca']],
    ['Porción y horarios', ['Pellets: unas 2 cucharadas al día', 'Aproximadamente 1 taza de verduras frescas al día', 'Repartir en la mañana y en la tarde']],
    ['Qué evitar', ['Chocolate, cebolla, ajo y papa cruda', 'Lácteos y comida humana procesada', 'Comida de conejo: no tiene la vitamina C que necesitan']],
    ['Cuidados', ['No fabrican vitamina C, hay que dársela en la dieta', 'Viven mejor en pareja', 'Jaula amplia, uñas cortas y revisión de dientes']]
  ]},
  { id: 'aves', n: 'Aves', e: '🦜', i: [
    ['Qué darle', ['Mezcla de semillas o pellets según la especie', 'Verduras de hoja y fruta fresca', 'Hueso de sepia para el calcio', 'Agua limpia todos los días']],
    ['Porción y horarios', ['Periquito: 1 a 2 cucharaditas de semillas al día', 'Canario: alrededor de 1 cucharadita al día', 'Verdura fresca a diario en cantidad pequeña']],
    ['Qué evitar', ['Aguacate (es tóxico para las aves)', 'Chocolate, cafeína, alcohol, sal y cebolla', 'Sartenes antiadherentes en la cocina: sus vapores son peligrosos']],
    ['Cuidados', ['Jaula amplia, lejos de corrientes de aire', 'Tiempo fuera de la jaula y juguetes', 'Taparla de noche para que descanse; si pasa mucho tiempo esponjada, ve al veterinario']]
  ]},
  { id: 'peces', n: 'Peces', e: '🐠', i: [
    ['Qué darle', ['Hojuelas o gránulos para su especie', 'Alimento congelado ocasional, como artemia', 'Vegetales para las especies herbívoras']],
    ['Porción y horarios', ['Solo lo que se coman en 2 minutos', '1 a 2 veces al día', 'Es mejor quedarse corto que pasarse']],
    ['Qué evitar', ['Pan y comida humana', 'Sobrealimentar: ensucia y daña el agua', 'Mezclar especies que no son compatibles']],
    ['Cuidados', ['Cambiar 20 a 30% del agua cada semana', 'Filtro y temperatura estables', 'Usar acondicionador de agua y no sobrepoblar el acuario']]
  ]},
  { id: 'tortugas', n: 'Tortugas', e: '🐢', i: [
    ['Qué darle', ['Acuáticas: pellets para tortuga, hojas verdes y algo de proteína', 'Terrestres: hojas verdes, verduras y flores comestibles', 'Calcio (sepia o suplemento)']],
    ['Porción y horarios', ['Juveniles: todos los días', 'Adultas: cada 2 días', 'Una porción del tamaño de su cabeza']],
    ['Qué evitar', ['Carne cruda y comida de perro o gato', 'Lechuga iceberg y alimentos procesados', 'Mucha fruta (solo como premio)']],
    ['Cuidados', ['Lámpara UVB y zona caliente para tomar sol', 'Agua limpia y espacio amplio', 'Lavarse las manos después de tocarla: pueden portar salmonela']]
  ]}
];

const tabsBox = document.getElementById('tabs');
const panelesBox = document.getElementById('paneles');
animales.forEach((a, k) => {
    const t = document.createElement('button');
    t.className = 'tab' + (k === 0 ? ' on' : '');
    t.dataset.panel = a.id;
    t.setAttribute('role', 'tab');
    t.textContent = a.e + ' ' + a.n;
    tabsBox.appendChild(t);

    const p = document.createElement('div');
    p.className = 'panel' + (k === 0 ? ' on' : '');
    p.id = a.id;
    p.innerHTML = a.i.map((s, n) => `<div class="acc-item${n === 0 ? ' open' : ''}">
        <button class="acc">${s[0]}<span class="flecha-acc">&#9660;</span></button>
        <div class="acc-body"><div class="acc-inner"><ul>${s[1].map(x => `<li>${x}</li>`).join('')}</ul></div></div></div>`).join('');
    panelesBox.appendChild(p);
});

/* ===== LISTAS DESPLEGABLES Y PESTAÑAS ===== */
document.addEventListener('click', e => {
    const acc = e.target.closest('.acc');
    if (acc) acc.parentElement.classList.toggle('open');

    const tab = e.target.closest('.tab');
    if (tab) {
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('on'));
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('on'));
        tab.classList.add('on');
        document.getElementById(tab.dataset.panel).classList.add('on');
    }
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
