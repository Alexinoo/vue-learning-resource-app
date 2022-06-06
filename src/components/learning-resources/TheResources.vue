<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')" :mode="storedResButtonMode">
            Stored Resources
        </base-button>


        <base-button @click="setSelectedTab('add-resource')" :mode="addResButtonMode">
            Add Resource
        </base-button>
    </base-card>

    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
    
</template>


<script>

import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {

    components: {

        StoredResources,
        AddResource,
    },

    data(){

        return {
            selectedTab : 'stored-resources',
            storedResources: [
                {
                    id: 'official-guide',
                    title: 'Official Guide',
                    description: 'The official Vue.js documentation',
                    link: 'https://vuejs.org/'
                },
                {
                    id: 'google',
                    title: 'Google',
                    description: 'Learn to google..',
                    link: 'https://google.com'
                },
            ],
        }
    },

    provide(){
        return {
            resources : this.storedResources,
            addResource: this.addResource,
            deleteResource: this.removeResource,
        }
    },

    computed : {
        storedResButtonMode(){
            return this.selectedTab === 'stored-resources' ? null : 'flat';
        } ,
        addResButtonMode(){
            return this.selectedTab === 'add-resource' ? null : 'flat';
        } ,
    },

    methods : {
        setSelectedTab( tab ){
            this.selectedTab = tab
        } ,

        addResource( title , description , url ){

            const newResource = {
                id : new Date().toISOString() ,
                title : title ,
                description : description ,
                link : url ,
            }

            this.storedResources.unshift(newResource)

            this.selectedTab = 'stored-resources'
        } ,

        removeResource(resourceID){

            // OPTION 1 - THIS WONT WORK BECAUSE filter returns a new Array ; And will try to overwrite the one existing in the memory and the new array will not update the resources injected in the other components

            // this.storedResources = this.storedResources.filter(resource => resource.id !== resourceID)
            // console.log(this.storedResources.length);


            // OPTION 2 - FINDBYINDEX  - Manipulates the array in memory and removes the array at specified index            
            
            // FIND INDEX OF THE RESOURCE THAT MATCHES WITH THE resourceID
            const resourceIndex = this.storedResources.findIndex(resource => resource.id === resourceID)

            // THEN USE SPLICE TO REMOVE IT -SPLICE WORKS BY REMOVING THE ITEM AT SPECIFIED INDEX AND THE SECOND PARAMETER SPECIFIES HOW MANY ITEM NEEDS TO BE REMOVED
            // WORKS JUST LIKE PUSHING/UNSHIFTING - MODIFIES THE EXISTING ARRAY
            this.storedResources.splice(resourceIndex , 1 )
        }
    }
}

</script>