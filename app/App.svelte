<script>
  import { showModal } from "svelte-native";
  const utilsModule = require("tns-core-modules/utils/utils");
  const appSettings = require("tns-core-modules/application-settings");
  import { onMount } from "svelte";
  import Article from "./modals/Article.svelte";

  let countryNumber = 0;
  let country = "us";
  $: {
    country = countryNumber == 0 ? "us" : "no";
    getData();
  }

  const api_key = "34b996cce1ac48cc9dcd5eff1460d6d8";

  const getData = () => {
    onMount(() => {
      //Vi må ha url med https, så putt en 's' etter http
      const url = `https://newsapi.org/v2/top-headlines?country=${country}&apiKey=${api_key}`;
      fetch(url)
        .then(response => response.json())
        .then(json => {
          articles = json.articles;
        })
        .catch(error => console.log(error));
    });
  };

  let articles = [];

  onMount(() => {
    getData();
  });

  const showArticle = async article => {
    await showModal({
      page: Article,
      props: {
        article: article
      }
    });
  };
</script>

<style>
  .top-bar {
    background-color: #fff;
    color: #333;
  }
  .container {
    width: 100%;
    background-color: #eee;
  }
  .articles {
    margin-top: 16;
    width: 100%;
    background-color: #eee;
  }
  .article {
    min-height: 100;
    padding: 24 16;
    margin: 0 16 2;
    background-color: #fff;
  }
  .white {
    color: #333;
  }
</style>

<page>
  <actionBar title="Bjørneposten" class="top-bar" />
  <stackLayout class="container">
    <segmentedBar
      selectedBackgroundColor="#fff"
      backgroundColor="#eee"
      color="#333"
      style="margin: 8 16;"
      bind:selectedIndex={countryNumber}>
      <segmentedBarItem title="Nyheter" />
      <segmentedBarItem title="Sport" />
    </segmentedBar>
    <scrollView>
      <stackLayout class="articles">
        {#each articles as article}
          <stackLayout on:tap={() => showArticle(article)} class="article">
            <image src={article.urlToImage} alt="cover" stretch="aspectFill" />
            <label
              textWrap={false}
              class="h2 text-center white"
              text={article.title} />
            <label class="body" text={article.author} />

          </stackLayout>
        {:else}
          <activityIndicator busy={true} />
        {/each}
      </stackLayout>
    </scrollView>
  </stackLayout>
</page>
