<script lang="ts">
    class Temperature {
        cel: number = $state(0)
        far: number = $state(32)

        get c(): number {
            return this.cel
        }

        set c(c: number) {
            this.cel = c
            this.far = c * (9/5) + 32
        }
        get f(): number {
			return this.far
		}

		set f(f: number) {
			this.far = f
			this.cel = (f - 32) * (5 / 9)
		}
    }

    const temperature = new Temperature()
</script>

<div class="container">
    <h1>Temperature Converter</h1>
    <input type="number" bind:value={temperature.c} /> Celcius &nbsp; === &nbsp;
    <input type="number" bind:value={temperature.f} /> Fahrenheit
</div>

<style>
    input[type='number'] {
        max-width: 15ch;
    }
</style>