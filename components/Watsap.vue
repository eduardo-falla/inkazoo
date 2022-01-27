<template>
   
	<a :href="`https://wa.me/+51925805923?text=¡Hola!%20quisiera%20información%20sobre%20las%20${this.cosa}%20de%20Inkazoo`" class="whatsapp" target="_blank"> <i class="fa fa-whatsapp whatsapp-icon"></i></a>

</template>

<script>
export default {
    name: "watsap",
    props: {
        cosa: {
            type: String,
            default() {
                return {};
            }
        },
   
    }
}
</script>

<style  >
@import "https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

.whatsapp {
  position:fixed;
  width:100px;
  height:100px;
  bottom:40px;
  right:40px;
  background-color:#25d366;
  color:#FFF;
  border-radius:50px;
  text-align:center;
  font-size:70px;
  z-index:100;
}

.whatsapp-icon {
  margin-top:13px;
}
</style>