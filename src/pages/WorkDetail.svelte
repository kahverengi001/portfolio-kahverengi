<script>
  import { route } from "routve";
  import { _, locale } from "svelte-i18n";

  import References from "../references.config";

  export let workID;

  if (
    typeof workID === "undefined" ||
    workID === null ||
    typeof References["works"][workID] === "undefined"
  ) {
    route("/error-404");
  }

  $: work =
    typeof workID === "undefined" ||
    workID === null ||
    typeof References["works"][workID] === "undefined"
      ? null
      : References["works"][workID][$locale];
</script>

<article class="mb-5">
  <h4 class="mb-4 text-light">
    <a href="/references#works">{$_("pages.work_details.work")}</a>
    ≫ {work ? work.companyName : ""}
  </h4>
  <img
    src="{work ? work.logoImage : ''}"
    alt="General Mobile Inc."
    class="rounded img-fluid" />
  <h5 class="my-4">{$_("pages.work_details.description")}</h5>
  <p>
    {@html work ? work.description : ""}
  </p>
  <ul class="list-inline text-muted">
    <li class="mb-2">
      <span class="mr-2 text-primary">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="20"
          height="20"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="feather feather-briefcase">
          <rect x="2" y="7" width="20" height="14" rx="2" ry="2"></rect>
          <path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"></path>
        </svg>
      </span>
      {$_("pages.work_details.position", {
        values: {
          position: work ? work.position : "",
          startDate: work
            ? work.endDate === ""
              ? work.startDate
              : $locale === "en"
              ? $_("pages.work_details.between") + work.startDate
              : work.startDate
            : "",
          endDate: work
            ? work.endDate === ""
              ? $_("pages.work_details.still")
              : work.endDate
            : "",
        },
      })}
    </li>
    <li>
      <span class="mr-2 text-primary">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="20"
          height="20"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="feather feather-map-pin">
          <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"></path>
          <circle cx="12" cy="10" r="3"></circle>
        </svg>
      </span>
      {work ? work.location.city : ""}, {work ? work.location.country : ""}
    </li>
  </ul>

  <h5 class="my-4">{$_("pages.work_details.used_technologies")}</h5>
  <ul class="list-inline">
    {#each work ? work.usedTechnologies : [] as technology, index (technology)}
      <li>- {technology}</li>
    {/each}
  </ul>
</article>
