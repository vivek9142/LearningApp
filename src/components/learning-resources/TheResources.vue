<template>
    <base-card>
    <!-- So if I add any props or event listeners to base button for example,
    they will fall through and will be applied to the root HTML element in base button's 
    template.which in this case is the native HTML button. -->
        <base-button 
            @click="setSelectedTab('stored-resources')" 
            :mode="storedResButtonMode"
        >Stored Resources</base-button>

        <base-button 
            @click="setSelectedTab('add-resource')"
            :mode="addResButtonMode"
        >Add Resource</base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>


<script>  
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
    components:{
        StoredResources,
        AddResource},
    data(){
        return {
            selectedTab:'stored-resources',
             storedResources:[
                {
                    id: 'official-guide', 
                    title:'Offical Guide',
                    description:"The Official Vue.js documentation",
                    link:'https://vuejs.org'
                },
                {
                    id: 'google', 
                    title:'Google',
                    description:"Learn to google...",
                    link:'https://google.com'
                }
            ]
        }
    },
    computed:{
    storedResButtonMode(){
      return this.selectedTab === 'stored-resources' ? null : 'flat';
        },
    addResButtonMode(){
      return this.selectedTab === 'add-resource' ? null : 'flat'
        }
    },
    provide(){
        return{
            resources:this.storedResources,
            addResource:this.addResource,
            removeResource:this.removeResource
        }
    },
    methods:{
        setSelectedTab(tab){
            this.selectedTab = tab;
        },
        addResource({title,description,link}){
            const newData = {
                id:new Date().toISOString(),
                title,
                description,
                link
            };

            this.storedResources.unshift(newData);
            this.selectedTab = 'stored-resources';
        },
        removeResource(resId){
            const resIndex = this.storedResources.findIndex(res => res.id === resId);
            this.storedResources.splice(resIndex,1);
        }
    }
}
</script>