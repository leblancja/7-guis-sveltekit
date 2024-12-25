<script lang="ts">

    type Option = 'one-way' | 'round-trip'
    function getDate() {
        const date = new Date()
        const [month, day, year] = date
            .toLocaleDateString('en-US', {year: 'numeric', month: '2-digit', day: '2-digit'})
            .split('/')
        return `${year}-${month}-${day}`
    }

    function handleSubmit(e: Event) {
        e.preventDefault()
        alert(`Booked a ${selectedOption}`)
    }

    let selectedOption = $state<Option>('one-way')
    let startDate = $state(getDate())
    let returnDate = $state(getDate())
</script>

<div class="container">
    <h1>Flight Booker</h1>
    <form onsubmit={handleSubmit}>
        <select bind:value={selectedOption}>
            <option value="one-way">one-way flight</option>
            <option value="round-trip">two-way flight</option>
        </select>
       
        <label>
            <p> Starting Date</p>
            <input type="date" bind:value={startDate} min={getDate()} required />
        </label>
        <label>
            <p> Return Date</p>
            <input type="date" bind:value={returnDate} min={getDate()} disabled={selectedOption !== 'round-trip'} required />
        </label>
    
        <button type="submit" disabled={!startDate || (selectedOption === 'round-trip' && returnDate < startDate)}>
            Book
        </button>
    </form>
</div>

<style>
    form p {
        margin-bottom: 0;
    }
</style>