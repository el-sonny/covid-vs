<script>
	export let options = {
		scale : 'logarithmic',
		dayZero : true,
		data : 'cases'
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

<ul>
	<li on:click={toggleDayZero}>
		<span class='text'>{options.dayZero ? 'Day Zero' : 'Natural Days'}</span>
		<span class='indicator'></span>
	</li>
	<li on:click="{toggleScale}" >
		<span class='text'>Scale : {options.scale} </span>
		<span class='indicator'></span>
	</li>

	<li on:click={toggleByRate}>
		<span class='text'>Data: {options.data} </span>
		<span class='indicator'></span>
	</li>

</ul>
<style>

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
		cursor:pointer;
		color:white;
		font-weight: bold;
		background-color: #555;
		padding:0 0 0 10px;

	}
	li .text{
		flex-grow: 1;
		text-align:left;
	}
	li .indicator{
		background-color:rgba(100,100,100,.5);
		color:white;
		width:20%;
		font-weight:bold;
	}
</style>