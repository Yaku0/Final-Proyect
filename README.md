const readlineSync = require('readline-sync');

class Viaje {
    constructor(tractomula, distanciaRecorrida) {
        this.tractomula = tractomula;
        this.distanciaRecorrida = distanciaRecorrida;
    }
}

class Carga {


    materialCarga = '';
    pesoMax = '';
    valorCarga = 0;
    rechazoBascula = false;

    constructor(materialCarga, pesoMax, valorCarga, rechazoBascula) {
        this.materialCarga = materialCarga;
        this.pesoMax = pesoMax;
        this.valorCarga = valorCarga;
        this.rechazoBascula = rechazoBascula;
    }
    calcularCostoTransporte(distancia) {

        return distancia * this.valorCarga;
    }

    calcularCostoTransporte(distancia) {
        let costoPorKg = 0;
        switch (this.materialCarga) { // Debes usar this.materialCarga en lugar de this.tipoDeCarga
            case 'Alimentos':
                costoPorKg = 10;
                break;
            case 'Hierro':
                costoPorKg = 20;
                break;
            case 'Cemento':
                costoPorKg = 30;
                break;
            case 'Electrodomesticos':
                costoPorKg = 40;
                break;
            case 'Tejas':
                costoPorKg = 50;
                break;
            default:
                throw new Error('Tipo de carga no válido');
        }

        const costoTotal = costoPorKg * this.pesoMax * (distancia / 100);
        return costoTotal;
    }
    
    calcularImpuestoTotal(distancia) {
        let impuestoPorKg = this.calcularImpuesto();
        let pesoEnKg = parseInt(this.pesoMax); // Convertir el peso máximo de la carga a kilogramos
        let impuestoTotal = impuestoPorKg * pesoEnKg * this.valorCarga * (distancia / 100);
        return impuestoTotal;
    }

    mostrarDatos() {
        console.log("Material de carga:", this.materialCarga);
        console.log("Peso máximo:", this.pesoMax);
        console.log("Valor de la carga:", this.valorCarga);
        console.log("Rechazo de la báscula:", this.rechazoBascula ? "Sí" : "No");
    }
    calcularImpuesto() {
        if (this.pesoMax <= 5) {
            return 0.05;
        } else if (this.pesoMax <= 10) {
            return 0.07;
        } else if (this.pesoMax <= 15) {
            return 0.12;
        } else {
            return 0.20;
        }
    }
}



let carga1 = new Carga('Alimentos', '5T', 1000, false);
let carga2 = new Carga('Hierro', '10T', 2000, false);
let carga3 = new Carga('Cemento', '15T', 3000, false);
let carga4 = new Carga('Electrodomesticos', '5T', 2500, false);
let carga5 = new Carga('Tejas', '7T', 3500, false);




class Tractomula {

    marca = '';
    #modelo = 0;// creacion de la mula
    #tipoDeMula = '';//Carroceria, tanque, etc
    #tipoDeCarga = '';// tipo1, tipo2, tipo3, etc
    pesoCarga = 0;// peso en toneladas
    pesoMinimo = 0;// peso minimo de carga
    pesoBascula = false;// pesaje total de la carga
    carga = 0;
    


    //Metodo constructor; nos permite proteger el programa para que no se creen objetos
    //sin los datos requeridos
    //en este metodo tambien podemos inicializar otro atributos, segun las necesidades que se tenga
    //Evita que se creen objetos aleatorios
    constructor(modelo, marca, tipoDeMula, tipoDeCarga, pesoCarga, pesoMinimo, pesoBascula = false, carga) {
        if (!modelo) {
            throw new Error('La Mula requiere un modelo');
        }

        if (!marca) {
            throw new Error('La Mula requiere una marca');
        }

        if (!tipoDeMula) {
            throw new Error('Se necesita saber que tipo de mula es');
        }

        if (!tipoDeCarga) {
            throw new Error('La Mula requiere un tipo de carga');
        }
        if (!pesoCarga) {
            throw new Error('Se necesita un peso permitido que sea mayor que 0');

        }
        if (!pesoMinimo) {
            throw new Error('Se requiere un peso minimo');

        }


        this.#modelo = modelo;
        this.#tipoDeCarga = tipoDeCarga;
        this.#tipoDeMula = tipoDeMula;
        this.pesoCarga = pesoCarga;
        this.pesoMinimo = pesoMinimo;
        this.marca = marca;
        this.pesoBascula = pesoBascula;
        this.carga = carga

        
    }

    
    mostrarDatos() {
        console.log("Modelo:", this.modelo);
        console.log("Marca:", this.marca);
        console.log("Tipo de mula:", this.tipoDeMula);
        console.log("Tipo de carga:", this.tipoDeCarga);
        console.log("Peso de carga:", this.pesoCarga);
        console.log("Peso mínimo:", this.pesoMinimo);
        console.log("Peso de la báscula:", this.pesoBascula ? "Sí" : "No");
        if (this.carga) {
            console.log("Puede cargar");
            this.carga.mostrarDatos();
        } else {
            console.log("No hay carga asignada.");
        }
        
    }
    //Encapsulamiento de modelo
    get modelo() {
        return this.#modelo;
    }

    //Encapsulamiento: proteccion del atributo: metodo setter
    set modelo(numeroDelModelo) {
        if (numeroDelModelo <= 0) {
            throw new Error(`El modelo no puede ser 0 o menor que 0`);
        }
        this.#modelo = numeroDelModelo;
    }

    //nuevo encapslamiento de TipoDeMula
    get tipoDeMula() {
        return this.#tipoDeMula;
    }

    //Encapsulamiento: proteccion del atributo: metodo setter TipoDeMula
    set tipoDeMula(nuevoTipoDeMula) {
        if (!isNaN(nuevoTipoDeMula)) {
            throw new Error(`La Mula debe cumplir con los requisitos del trailer`);
        }
        this.#tipoDeMula = nuevoTipoDeMula;
    }

    //Encapsulamiento de tipoDeCarga
    get tipoDeCarga() {
        return this.#tipoDeCarga;
    }

    //Encapsulamiento: proteccion del atributo: metodo setter
    set tipoDeCarga(nuevoTipoDeCarga) {
        if (nuevoTipoDeCarga < 1 || nuevoTipoDeCarga > 5) {
            throw new Error('El tipo de mula debe tener un valor de 1 a 5');
        }
        this.#tipoDeCarga = nuevoTipoDeCarga;
    }
    // creamos unos metodos necesarios
    imprimirEstadoPesaje() {

        if (this.pesoBascula === false) {
            console.log(`Peso de la báscula: No`);
        } else if (this.pesoBascula === true) {
            console.log(`Peso de la báscula: Si`);
        }
    }
    compararPesoCarga(otraMula) {
        if (this.pesoCarga < otraMula.pesoCarga) {
            console.info(`La Mula ${this.marca} es mas pesada que ${otraMula.marca}`);

        } else if (this.pesoCarga > otraMula.pesoCarga) {
            console.info(`La Mula ${this.marca} es más pesada que ${otraMula.marca}`);

        }
        else {
            console.info(`La Mula ${otraMula.marca} es mas pesada que ${this.marca}`);
        }

    }

    validarPesoMax() {

        if (this.pesoCarga < 5) {
            this.pesoBascula = 'La carga es demasiado liviana para la Mula';
            return;
        }

        if (this.pesoCarga > 100) {
            this.pesoBascula = 'La carga es demasiada pesada para la Mula';
            return;
        }
        this.pesoBascula = null; // Reinicia el estado de la bascula
        console.info('La Mula está disponible para cargar.');
        return true;
    }

    validarPesoMinimo() {

        if (this.pesoCarga <= 2) {
            this.pesoMinimo = 'La carga minima debe ser mayor o igual que 2';
            return;
        }

        else if (this.pesoCarga >= 100) {
            this.pesoMinimo = 'La carga es demasiada pesada para la Mula';
            return;
        }

    }
    
    }
    
    let cargas = [carga1, carga2, carga3, carga4, carga5];
    
    // Calcular el impuesto total para cada tipo de carga
    let impuestosTotales = {};
    for (let carga of cargas) {
        let tipoDeCarga = carga.materialCarga;
        if (!impuestosTotales[tipoDeCarga]) {
            impuestosTotales[tipoDeCarga] = 0;
        }
        let distanciaRecorrida = 200;
        impuestosTotales[tipoDeCarga] += carga.calcularImpuestoTotal(distanciaRecorrida);
    }

    // Encontrar el tipo de carga con el impuesto total más alto
    let tipoMasImpuestos;
    let impuestosMasAltos = 0;
    for (let tipoDeCarga in impuestosTotales) {
        if (impuestosTotales[tipoDeCarga] > impuestosMasAltos) {
            impuestosMasAltos = impuestosTotales[tipoDeCarga];
            tipoMasImpuestos = tipoDeCarga;
        }
    
    }
    
    

// creamos nuestras variables necesarias con sus resoectivas clases y atributos
let tractomula1 = new Tractomula(2001, 'Volvo', 'Carroceria', 'tipo1', 5, 2, false, carga1);
let tractomula2 = new Tractomula(2002, 'Mercedes', 'Camabaja', 'tipo2', 10, 2, false, carga2);
let tractomula3 = new Tractomula(2003, 'Iveco', 'tanque', 'tipo3', 5, 2, false, carga3);
let tractomula4 = new Tractomula(2004, 'Eagle', 'Plancha', 'tipo4', 13, 2, false, carga4);
let tractomula5 = new Tractomula(2005, 'Mazda', 'Remolque', 'tipo5', 12, 2, false, carga5);

let viaje1 = new Viaje(tractomula1, 200); // Suponiendo que tractomula1 es la tractomula utilizada en este viaje y la distancia recorrida es 200 km
let viaje2 = new Viaje(tractomula2, 300); 
let viaje3 = new Viaje(tractomula3, 250);
let viaje4 = new Viaje(tractomula3, 240);
let viaje5 = new Viaje(tractomula3, 210);

// Luego, puedes almacenar estos objetos de viaje en un array
let viajesRealizados = [viaje1, viaje2, viaje3, viaje4, viaje5];

let calcularCostoTransporte = new Carga();
function calcularDineroTotalRecaudado(viajes) {
    let dineroTotal = 0;
    for (let viaje of viajes) {
        let carga = viaje.tractomula.carga;
        let distanciaRecorrida = viaje.distanciaRecorrida;
        let costoTransporte = carga.calcularCostoTransporte(distanciaRecorrida);
        console.log(`Costo de transporte para ${carga.materialCarga}: ${costoTransporte}`);
    }
    return dineroTotal;
}

// Suponiendo que tienes un array de objetos de viaje
let dineroRecaudado = calcularDineroTotalRecaudado(viajesRealizados);



if (tractomula1 == tractomula2) {
    console.log('Las Tractomulas 1 y 2 son iguales')
}
else {
    console.log('Las Tractomulas 1 y 2 son diferentes pero tienen el mismo ESTADO');
}

console.info('Las Tractomulas son tienen diferentes instancias pero tienen el mismo estado');
console.info({ tractomula1 });
console.info({ tractomula2 });

console.info('La Tractomula se asigna a la Tractomula2');

tractomula1 = tractomula2
if (tractomula1 == tractomula2) {
    console.log('La Tactomula 1 y 2 son igual porque son el mismo OBJETO')
}
else {
    console.log('La Tractomula 1 y 2 son diferentes')
}
//encapsulamientoM protefer los atributos con metodos GET Y SET;

tractomula2.modelo = 2002;

console.info({ tractomula1 });
console.info({ tractomula2 });

console.info('Ahora se instancia de nuevo tractomula2');
tractomula1 = new Tractomula(2001, 'Volvo', 'Carroceria', 'tipo1', 5, 2)
console.info({ tractomula1 });
console.info({ tractomula2 });


class TractomulaEspecial extends Tractomula {
    constructor(modelo, marca, tipoDeMula, tipoDeCarga, pesoCarga, pesoMinimo, pesoBascula, carga, tipoEspecial) {
        super(modelo, marca, tipoDeMula, tipoDeCarga, pesoCarga, pesoMinimo, pesoBascula, carga);
        this.tipoEspecial = tipoEspecial;
    }

    calcularCostoAdicional() {
        let costoAdicional = 0;
        switch (this.tipoEspecial) {
            case 'Resistente':
                costoAdicional = 5000;
                return costoAdicional;
            case 'Veloz':
                costoAdicional = 3000;
                return costoAdicional;
            default:
                costoAdicional = 0;
                return costoAdicional;
        }
    

}
    }


tractomula1.validarPesoMax();
tractomula2.validarPesoMax();
tractomula3.validarPesoMax();
tractomula4.validarPesoMax();
tractomula5.validarPesoMax();


tractomula1.validarPesoMinimo();
tractomula2.validarPesoMinimo();
tractomula3.validarPesoMinimo();
tractomula4.validarPesoMinimo();
tractomula5.validarPesoMinimo();

tractomula1.mostrarDatos();
tractomula2.mostrarDatos(); 
tractomula3.mostrarDatos(); 
tractomula4.mostrarDatos();
tractomula5.mostrarDatos();

console.log("Dinero total recaudado por concepto de viajes realizados:", dineroRecaudado);

console.log("El tipo de carga que genera más impuestos es:", tipoMasImpuestos);


