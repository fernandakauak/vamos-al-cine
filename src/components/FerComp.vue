<script setup></script>

<template>
    <header>
        <h2>Cine PokeMetálicos <span>presenta</span>:</h2>
        <h1>Carrie</h1>
    </header>
    <section>
        <aside>
            <h2>Tu compra</h2>
            <p>Butacas elegidas: <span>Ninguna todavía</span></p>
            <label for="salapeli">Tipo de Función:</label>
            <select name="" id="salapeli">
                <option value="">2D - Normal</option>
                <option value="">3D — (+$1.500 por butaca)</option>
            </select>
            <label for="retiro">Nombre de quién retira*:</label>
            <input type="text" id="retiro" placeholder="Tu nombre">

            <h3>Butacas <span>$0</span></h3>
            <h2>Total <span>$0</span></h2>

            <button>Elige butacas y escribe tu nombre</button>
        </aside>
        <article>
            <h1>Función Especial: 1 de Agosto</h1>

            <div>Pantalla</div>

            <table class="butacas">
                <tr>
                    <th>A</th>
                    <td>1</td>
                    <td class="elegido">2</td>
                    <td>3</td>
                    <td class="ocupado">4</td>
                    <td>5</td>
                    <td>6</td>
                    <td>7</td>
                    <td>8</td>
                </tr>
                <tr>
                    <th>B</th>
                    <td>1</td>
                    <td>2</td>
                    <td>3</td>
                    <td>4</td>
                    <td>5</td>
                    <td>6</td>
                    <td>7</td>
                    <td>8</td>
                </tr>
                <tr>
                    <th>C</th>
                    <td>1</td>
                    <td>2</td>
                    <td>3</td>
                    <td>4</td>
                    <td>5</td>
                    <td>6</td>
                    <td>7</td>
                    <td>8</td>
                </tr>
                <tr>
                    <th>D</th>
                    <td>1</td>
                    <td>2</td>
                    <td>3</td>
                    <td>4</td>
                    <td>5</td>
                    <td>6</td>
                    <td>7</td>
                    <td>8</td>
                </tr>
                <tr>
                    <th>E</th>
                    <td>1</td>
                    <td>2</td>
                    <td>3</td>
                    <td>4</td>
                    <td>5</td>
                    <td>6</td>
                    <td>7</td>
                    <td>8</td>
                </tr>
            </table>
        </article>
        
    </section>

</template>

<style scoped>
    section {
        width: 96%;
        display: flex;
        gap: 2%;
        padding: 2%;
    }

    article {
        width: 76%;
        margin: 0;
        background-color: red;
    }

    aside {
        width: 20%;
        border-radius: 15px;
        background-color: darkslategrey;
        color: white;
        padding: 2%;
    }

    .butacas {
        width: 80%;
        margin: auto;
    }

    .butacas th {
        width: 5%;
    }

    .butacas tr {
        margin: 1%;
    }

    .butacas td {
        padding: 2%;
        background-color: blueviolet;
        color: white;
        border-radius: 10px;
    }
    
    .butacas td.elegido {
        background-color: green;
        font-weight: bold;
    }

    .butacas td.ocupado {
        background-color: darkslategray;
        color: white;
    }
</style>