<script>
  import { closeModal } from "svelte-native";
  export let article;
</script>

<style>
  .container {
    background-color: #eee;
  }
  .article {
    background-color: #fff;
    margin: 16 16 0 16;
  }
  .title {
    color: #333;
  }
  .author {
    color: #444;
    font-size: 12 !important;
  }
  .description {
    font-size: 16;
    color: #333;
    font-weight: bold;
    margin-bottom: 8;
  }
  .content {
    font-size: 16;
    color: #333;
    margin-bottom: 24;
  }
  .article-content {
    padding: 8 16;
  }
</style>

<frame>
  <page>
    <scrollView class="container">
      <stackLayout class="article">
        <image
          src={article.urlToImage}
          alt="article cover image"
          stretch="aspectFill" />
        <stackLayout class="article-content">

          <label textWrap="true" class="h1 title" style="padding-bottom: 4;">
            {article.title}
          </label>

          <label
            textWrap="true"
            class="author"
            text={'Oppdatert: ' + article.publishedAt}
            style="padding-bottom: 8;" />

          <label
            textWrap="true"
            class="author"
            style="padding-bottom: 8;"
            text={'Forfatter: ' + article.author} />

          <label textWrap="true" class="body description">
            {article.description}
          </label>

          <label textWrap="true" class="body content">{article.content}</label>
          <button on:tap={() => closeModal()} text="Tilbake" />

        </stackLayout>
      </stackLayout>
    </scrollView>
  </page>
</frame>
