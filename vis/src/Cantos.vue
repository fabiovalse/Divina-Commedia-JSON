<template>
  <div class="cantos" v-if="selection !== undefined">
    <div class="title">{{title}}</div>
    <div class="tercet" v-for="(tercet, i) in selection.children">
      <div class="line"
           v-for="line in tercet.children"
           v-html="text_search !== undefined ? get_highlighted(text_search, line.text) : line.text">
      </div>
      <div class="number" v-if="tercet.children.length == 3">{{3*(i+1)}}</div>
    </div>
  </div>
</template>

<script lang="coffee">
export default {
  
  computed:
    selection: () -> @$store.state.path
    text_search: () -> @$store.state.text_search
    title: () -> "#{@$router.currentRoute.params.book} - #{@selection.name}"

  mounted: () ->
    offset = 0
      
    if @$store.state.tercet.number > 1
      offset = document.querySelectorAll('.tercet')[@$store.state.tercet.number-1].getBoundingClientRect().top-63
      console.log offset

    window.scrollTo 0, offset

  methods: 
    get_highlighted: (text_search, text) ->
      re = new RegExp text_search, 'gi'
      return text.replace(re, "<span class='highlighted'>#{text_search}</span>")

}
</script>

<style scoped>
.cantos {
  padding-top: 40px;
  background: var(--main-background-color);
}
.cantos > * {
  width: 350px;
  margin: auto;
  margin-bottom: 20px;
}
.cantos .title {
  font-size: 18px;
  font-weight: bold;
}
.cantos .tercet {
  position: relative;
}
.cantos .tercet .number {
  position: absolute;
  right: -15px;
  bottom: 0px;
  color: gray;
  font-size: 13px;
}
</style>
<style>
.cantos .tercet .line .highlighted {
  background: var(--selection-color);
}
</style>