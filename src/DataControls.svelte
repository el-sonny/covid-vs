<script>
	export let options = {
		scale : 'logarithmic',
		dayZero : true,
		data : 'cases',
		zoom : 100,
	}

	function toggleDayZero(){
		options.dayZero = !options.dayZero;
	}
	function toggleByRate(){
		options.data = options.data === 'rates' ? 'cases' : 'rates';
	}
	function toggleScale(){
		options.scale = options.scale === 'linear' ? 'logarithmic' : 'linear';
	}
</script>
<h3>Graph Controls</h3>
<ul>
	
	<li on:click={toggleDayZero}>
		<span class='text'>{options.dayZero ? 'Day Zero' : 'Natural Days'}</span>
		<span class='indicator'>
			{#if options.dayZero}
				<i class='material-icons on'>toggle_on</i>
			{:else}
				<i class='material-icons off'>toggle_off</i>
			{/if}
		</span>
	</li>
	<li on:click="{toggleScale}" >
		<span class='text'>Scale: {options.scale} </span>
		<span class='indicator'>
			{#if options.scale === 'logarithmic'}
				<i class='material-icons on'>toggle_on</i>
			{:else}
				<i class='material-icons off'>toggle_off</i>
			{/if}
		</span>
	</li>

	<li on:click={toggleByRate}>
		<span class='text'>Data: {options.data} </span>
		<span class='indicator'>
			{#if options.data === 'cases'}
				<i class='material-icons on'>toggle_on</i>
			{:else}
				<i class='material-icons off'>toggle_off</i>
			{/if}
		</span>
	</li>
	
	<li class='range-li'>		
		<span class='text range-container'>
			<i class='material-icons'>search</i>
			<input type='range' min='1' max='100' bind:value={options.zoom} class='range' />
		</span>
		<span clas='indicator'>{options.zoom}%</span>
	</li>
</ul>
 


<style>
	h3{
		margin:10px 5px;
		text-align: center;
	}
	ul{
		margin:10px 5px;
		padding:0 20px;
	}
	li{
		display:flex;
		list-style: none;
		margin-bottom:5px;
		text-align: center;
		height:40px;
		line-height:40px;
		width:100%;
		justify-content: flex-end;
		color:#444;
		font-weight: bold;
		padding:0 0 0 10px;
		cursor: pointer;

	}
	li .text{
		flex-grow: 1;
		text-align:left;
	}
	li .indicator{
		width:20%;
		font-weight:bold;
		line-height:38px;
	}
	li .indicator i{
		font-size: 32px;
		line-height:40px;
	}
	li .indicator .on{
		color:#016FB9;
	}
	li .range-container{
		padding:0 17px 0 0;
		display:flex;
	}
	li .range-container .material-icons{		
		line-height: 41px;
		margin-right:5px;
	}
	li.range-li{
		padding:10px 10px 0 0;
	}
	li .range{
		width:100%;
		flex-grow: 1;

	}
	
</style>