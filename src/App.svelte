<script lang="ts">
    import artist_logo from './assets/llwll.jpg'

    const links = [
        {url: 'https://twitter.com/llwlll__/', label: 'Twitter', icon: 'icons/twitter.svg'},
        {url: 'https://www.patreon.com/llwll/', label: 'Patreon', icon: 'icons/patreon.svg'},
        {url: 'https://llwll.bandcamp.com/', label: 'Bandcamp', icon: 'icons/bandcamp.png'},
        {url: 'https://open.spotify.com/artist/6JwRFnmMxmWcYribpIJbcS', label: 'Spotify', icon: 'icons/spotify.png'},
        {url: 'https://soundcloud.com/llwll', label: 'Soundcloud', icon: 'icons/soundcloud.png'},
        {url: 'https://www.instagram.com/llwlll_/?hl=en', label: 'Instagram', icon: 'icons/instagram.png'},
    ]


    function onLinkClick(url: string){
        window.open(url, "_blank")
    }


</script>

<main class="main-content">

    <div class="artist-content">

        <div class="artist-image">
            <img alt="llwll image" src={artist_logo} />
        </div>
    
    
        <div class="artist-links">
            {#each links as link }
    
            <div class="link" on:click={() => onLinkClick(link.url)}>
                <img src={link.icon} alt={link.label}>
                <span>{link.label}</span>
            </div>
                
            {/each}
    
        </div>
    

    </div>


    
</main>

<style lang="scss">

    .artist-image{
        display: flex;
        justify-content: center;
        img{
            height: 225px;
            border-radius: 50%;
            border: 2px solid black;
            box-shadow: rgba(255, 255, 255, 0.1) 0px 1px 1px 0px inset, rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px;
        }
    }

    .main-content{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
    }

    .artist-content{
        display: flex;
        flex-direction: column;
        gap: 50px;
    }

    .artist-links{
        display: flex;
        flex-direction: column;
        gap: 15px;
        
        justify-content: center;
        align-items: center;

        .link{
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 50px;
            cursor: pointer;
            width: 250px;

            border: 2px solid black;

            border-radius: 15px;
            padding: 25px;
 
            img{
                height: 25px;
            }

            span{

            }

        }


    }

</style>
