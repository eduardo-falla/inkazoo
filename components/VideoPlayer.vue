<template>
    <div>
        <video ref="videoPlayer" class="video-js" ></video>
    </div>
</template>

<script>
import videojs from 'video.js';

export default {
    name: "VideoPlayer",
    props: {
        fuente: {
            type: String,
            default() {
                return {};
            }
        }
    },
    data() {
        return {
            player: null
        }
    },
    mounted() {
        this.player = videojs(this.$refs.videoPlayer,
        
        
        {
				muted:true,
                autoplay: true,
                loop:true,
                fluid: true,
				controls: false,
				sources: [
					{
						src:
							this.fuente,
						  type: "video/mp4"
					}
				]
			}
        
        
        
        )
    },
    beforeDestroy() {
        if (this.player) {
            this.player.dispose()
        }
    }
}
</script> 