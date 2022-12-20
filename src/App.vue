<template>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<main>
  <h1><span id="p3">P3</span> Tablå</h1>
  <div className='schedule'>
    <ul id="episode-list"></ul>
  </div>
</main>
</html>
</template>

<script>

	const url = "http://api.sr.se/api/v2/scheduledepisodes?channelid=164";
  fetch(url)
  .then(response=>response.text())
  .then(data=>{
    let parser = new DOMParser();
    let xml = parser.parseFromString(data, "text/xml");
    console.log(xml);
    buildTableau(xml);
	});

	function buildTableau(x) {
		const episodeList = document.getElementById('episode-list');
		let episodes = x.getElementsByTagName('scheduledepisode');
		let epiTitle = x.getElementsByTagName('title');
		let epiDesc = x.getElementsByTagName('description');
		let epiStart = x.getElementsByTagName('starttimeutc');
		
		for(let i = 0; i < episodes.length; i++){
			const episode = document.createElement('li');
			episode.className = 'scheduled-episode';
			const info = document.createElement('div');
			info.className = 'episode-info';
			const time = document.createElement('div');
			time.className = 'start-time';
			const title = document.createElement('h3');
			title.className = 'episode-title';
			const descHolder = document.createElement('div')
			descHolder.className = 'desc-holder'
			const desc = document.createElement('p');
			desc.className = 'episode-desc';
			desc.style.display = 'none';
			
			const toggle = document.createElement('button');
			toggle.className = 'toggle-desc';
			toggle.addEventListener('click', () => {
				if (desc.style.display === "none") {
					desc.style.display = "block";
					toggle.innerHTML = "-";
				} else {
					desc.style.display = "none";
					toggle.innerHTML = "+";
				}
			});
			
			time.innerText = epiStart[i].innerHTML.slice(11, -4);
			title.innerText = epiTitle[i].innerHTML;
			desc.innerText = epiDesc[i].innerHTML;
			toggle.innerHTML = "+";

			episodeList.appendChild(episode);
			episode.appendChild(time);
			episode.appendChild(info);
			info.appendChild(title);
			info.appendChild(descHolder);
			descHolder.appendChild(toggle);
			descHolder.appendChild(desc);
		}
	}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #3c3e3d;
  margin-top: 40px;
}

body {
  background-color: rgb(241, 241, 241);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 1rem;
  color: #3c3e3d;
}

.schedule {
  width: 65vh;
  background-color: white;
  border-radius: 16px;
}

ul{
  margin-block-start: 0;
  padding-inline-start: 0;
}

#episode-list {
  padding-bottom: 2rem;
  text-align: left;
}

li {
  list-style: none;
  padding: 1.3rem 1.3rem 0;
}

.scheduled-episode,
.desc-holder {
  display: flex;
  align-items: baseline;
}

.episode-info,
.episode-desc {
  padding-left: 1rem;
  margin: 0;
}

button {
  width: 2rem;
  font-size: 1.3rem;
  color: #fff;
  background-color: #3c3e3d;
  border: none;
  border-radius: 45px;
  box-shadow: 0px 2px 3px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease 0s;
  cursor: pointer;
  outline: none;
  vertical-align: top;
}

#p3 {
  background-color: #2EE59D;
  color: white;
  padding: 1rem;
  border-radius: 8px;
}
</style>
