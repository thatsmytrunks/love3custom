[Return to Home](README.md)

# Every Sprite

Here are previews of all the sprites in the game:

<div id="sprite-container"></div>

<style>
  /* ... CSS styles ... */
</style>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const spriteContainer = document.getElementById('sprite-container');
  const repoUrl = 'https://api.github.com/repos/thatsmytrunks/love3custom/contents/docs/images/everySprite';
  const proxyUrl = 'https://cors-anywhere.herokuapp.com/'; // Example proxy server

  fetch(proxyUrl + repoUrl) // Use the proxy server for the API request
    .then(response => response.json())
    .then(data => {
      data.forEach(file => {
        if (file.type === 'file' && file.name.endsWith('_0.png')) {
          const spriteName = file.name.replace('_0.png', '');
          const imageUrl = file.download_url.replace('https://raw.githubusercontent.com/', 'https://thatsmytrunks.github.io/love3custom/'); // Adjust image path

          const spriteItem = document.createElement('div');
          spriteItem.className = 'sprite-item';

          const image = document.createElement('img');
          image.src = imageUrl;
          image.alt = spriteName;

          const name = document.createElement('p');
          name.textContent = spriteName;

          spriteItem.appendChild(image);
          spriteItem.appendChild(name);
          spriteContainer.appendChild(spriteItem);
        }
      });
    })
    .catch(error => console.error('Error fetching sprite data:', error));
});
</script>