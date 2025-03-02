[Return to Home](README.md)

# Every Sprite

Here are previews of all the sprites in the game:

<div id="sprite-container"></div>

<style>
#sprite-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}

.sprite-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 10px;
  text-align: center;
}

.sprite-item img {
  max-width: 100px;
  max-height: 100px;
}
</style>

<script>
window.onload = function() { // Ensure the script runs after the page loads
  const spriteContainer = document.getElementById('sprite-container');
  const repoUrl = 'https://api.github.com/repos/thatsmytrunks/love3custom/contents/docs/images/everySprite';

  fetch(repoUrl)
    .then(response => response.json())
    .then(data => {
      data.forEach(file => {
        if (file.type === 'file' && file.name.endsWith('_0.png')) {
          const spriteName = file.name.replace('_0.png', '');
          const imageUrl = file.download_url;

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
};
</script>