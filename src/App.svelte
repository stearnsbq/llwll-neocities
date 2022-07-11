<script lang="ts">
    import artist_logo from './assets/llwll.jpg'

    const links = [
        {url: 'https://twitter.com/llwlll__/', label: 'Twitter', icon: 'icons/twitter.svg'},
        {url: 'https://www.patreon.com/llwll/', label: 'Patreon', icon: 'icons/patreon.svg'},
        {url: 'https://llwll.bandcamp.com/', label: 'Bandcamp', icon: 'icons/bandcamp.svg'},
        {url: 'https://open.spotify.com/artist/6JwRFnmMxmWcYribpIJbcS', label: 'Spotify', icon: 'icons/spotify.svg'},
        {url: 'https://soundcloud.com/llwll', label: 'Soundcloud', icon: 'icons/soundcloud.svg'},
    ]


</script>

<main class="main-content">

    <div class="artist-content">

        <div class="artist-image">
            <img alt="llwll image" src={artist_logo} />
        </div>
    
    
        <div class="artist-links">
            {#each links as link }
    
            <div class="link">
                <img src={link.icon} alt={link.label}>
                <a href={link.url}>{link.label}</a>
            </div>
                
            {/each}
    
        </div>
    

    </div>


    
</main>

<style lang="scss">

    .artist-image{
        img{
            border-radius: 50%;
        }
    }

    .main-content{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
    }

    .artist-links{

        .link{
            img{
                height: 25px;
            }
        }


    }

</style>
