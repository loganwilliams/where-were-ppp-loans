<template>
  <div class="story">
    <Title text="Where were paychecks protected?" />
    <div class="w3 l1 p">
      Recently the Small Business Administration released a very interesting
      dataset of of loans issued under the 2020 Paycheck Protection Plan, after
      a number of FOIA requests. These loans, issued to small* businesses are
      forgivable (that is, “free money”) under a number of circumstances. I
      mapped this data to the ZIP Code level in California, and was struck by
      how starkly patterns of urban equity were revealed. To better compare
      disparate neighborhoods, I limited the loans I looked at to those for
      restaurants*, some of the most common and universal businesses.
      <div class="float left caption">
        *&lt; 500 employees<br /><br /><br /><br /><br /><br />*NAICS codes
        722511 and 722513<br /><br /><br /><br /><br /><br />
      </div>
    </div>

    <div class="w4 l0 p">
      Overwhelmingly, restaurants in wealthy and white neighborhoods had
      disproportionate access to capital through the program, and some ZIP codes
      received 20-50x more aid than similar places just miles away. This isn’t
      necessarily surprising &mdash; it is logical to expect that
      under-resourced areas, especially under-banked areas, would have less
      ability to apply for these loans. However, it does illustrate how policies
      and programs that ignore existing inequities can frequently perpetuate
      them.
    </div>

    <div class="w4 l0 p">
      Below, I show some of what I noticed in the data, starting with Los
      Angeles. You can also
      <a href="#interactive">jump straight to an interactive map</a> to explore
      for yourself.
    </div>
    <Map
      classString="w4 h3"
      :bbox="[-118.744354, 33.684354, -118.02475, 34.197037]"
      :center="[-118.21, 33.98]"
      :zoom="9"
      variable="pct_with_loan"
      title="Percent of restaurants with PPP loan*"
      staticImage="1.jpg"
    />
    <div class="w4 caption">
      *All loans are tallied by ZIP codes, and divided by the number of
      establishments in that ZIP Code according to the US Census ZIP Code
      Business Patterns dataset from 2017 (latest available.) Both datasets are
      filtered by the NAICS codes 722511 and 722513. ZIP Codes that do not
      correspond to a tabulation area (i.e., P.O. boxes) are assigned to the
      area that contains that post office. PPP loan data is from the Small
      Business Administration and contains user-entered data.
    </div>

    <div class="w2 l0 p">
      Clear patterns of regional inequity are visible in the map of Los Angeles,
      most notably between South LA and the Hollywood Hills. These ZIP Code
      tabulation areas (ZCTAs) defined by the census may not be great for
      statistical analysis*, but at least they carry emotional resonance. Let's
      take a closer look at the two neighborhoods outlined on the map.

      <div class="float">
        * ZIP Codes aren’t designed for geospatial analysis — in fact, they’re
        not even shapes, just lists of addresses! Because the shapes are
        arbitrary and ornate, they are vulnerable to the “Modified Area Unit
        Problem.”
      </div>
    </div>
    <div class="w4p share align-bottom">
      <div class="w2">
        <div class="w2" style="height: calc(16px * 30)">
          <img src="../assets/90069.jpg" />
        </div>

        <div class="w2 p-no-leading">
          <strong>90069: West Hollywood, CA</strong><br />
          117 restaurants<br />
          87 PPP loans to restaurants<br />
          74% loan access<br />
          $243,000 loaned per restaurant<br />
          $28.4 million loaned total<br />
          $92,952 median income<br />
          42 median age<br />
          80% white<br />
          9% Hispanic<br />
        </div>
      </div>

      <div class="w2p">
        <div class="w2p" style="height: calc(16px * 30)">
          <img src="../assets/90255.jpg" />
        </div>

        <div class="w2p p-no-leading">
          <strong>90255: Huntington Park, CA</strong><br />
          114 restaurants<br />
          32 PPP loans to restaurants<br />
          28% loan access<br />
          $14,000 loaned per restaurant<br />
          $1.5 million loaned total<br />
          $42,581 median income<br />
          29 median age<br />
          2% white<br />
          97% Hispanic<br />
        </div>
      </div>
    </div>

    <div class="l1 w3 p">
      Despite being only a few miles from each other, in <strong>90069</strong>,
      restaurants were 3x as likely to recieve a PPP loan, and the total amount
      loaned was nearly 20x larger. Just as stark are the demographic and
      socioeconomic differences: while <strong>90069</strong> is 80% white,
      <strong>90255</strong> is 97% Hispanic.
    </div>

    <div class="w3 p l1">
      A related statistic I derived from the PPP dataset is the total amount
      loaned in a ZIP code divided by each restaurant in that ZIP code. This
      average amount loaned per restaurant breaks down along roughly the same
      sextiles; but the distribution is even more intensely skewed towards
      wealthy areas.* This could be due to variation in restaurant size or
      ownership, as discussed below, but regardless, accurately shows how new
      money has flowed to certain neighborhoods more than others.

      <div class="float left caption">
        *that is, the highest sextile has a greater relative amount of the total
        loan value, then of the total number of loans
      </div>
    </div>

    <div class="share">
      <Map
        classString="w3 h3"
        variable="loan_per_company"
        title="PPP loan per restaurant"
        :bbox="[-118.744354, 33.684354, -118.02475, 34.197037]"
        :center="[-118.294, 33.98]"
        :zoom="9"
        staticImage="2.jpg"
      />
    </div>

    <div class="share">
      <Map
        classString="w2 h2"
        variable="black_and_hispanic"
        :bbox="[-118.744354, 33.684354, -118.02475, 34.197037]"
        :center="[-118.294, 33.98]"
        :zoom="9"
        title="Percent Black and Hispanic"
        legendStyle="below"
        staticImage="3.jpg"
      />

      <Map
        classString="w2 h2 below"
        variable="med_income"
        :bbox="[-118.744354, 33.684354, -118.02475, 34.197037]"
        :center="[-118.294, 33.98]"
        :zoom="9"
        title="Median income"
        legendStyle="below"
        staticImage="4.jpg"
      />
    </div>

    <div class="w4 p">
      The correlation with low-income neighborhoods, and Black and Hispanic
      majority neighborhoods, is noticeable and widespread, though not
      universal.
    </div>

    <Map
      classString="w4 h3"
      :center="[-122.1, 37.65]"
      :zoom="8.5"
      title="Percent of restaurants with PPP loan"
      variable="pct_with_loan"
      staticImage="5.jpg"
    />

    <div class="w3 l1 p">
      The prosperous Bay Area has mostly high access to PPP capital, but
      under-resourced pockets are still visible in places like Richmond, East
      Oakland, and Daly City, as well as the rapidly gentrifying peninsula
      enclaves of East Palo Alto and North Fair Oaks. Towards the north-east,
      two distinct waves of sprawl over the hills are visible: the wealthy
      neighborhoods around the Diablo foothills and the recently developed Delta
      to its north.
    </div>

    <div class="share">
      <div class="w2">
        <Map
          classString="w2 h2"
          :center="[-122.1811, 37.4688]"
          :zoom="10.5"
          title="Percent of restaurants with PPP loan"
          variable="pct_with_loan"
          :hideLegend="true"
          staticImage="6.jpg"
        />
        <div class="caption">
          <strong>East Palo Alto.</strong> Above, percent of restaurants with
          PPP loans. Below, percent of population that is Black or Hispanic.
          North Fair Oak's unique enclave of affordability is obscured in ZIP
          Code Tabulation areas due to its small size. The surrounding
          neighborhoods abut Palo Alto and Stanford's country-club like campus.
        </div>
        <Map
          classString="w2 h2"
          :center="[-122.1811, 37.4688]"
          :zoom="10.5"
          title="Percent Black and Hispanic"
          variable="black_and_hispanic"
          legendStyle="below"
          staticImage="7.jpg"
        />
      </div>
      <div class="w2">
        <Map
          classString="w2 h2"
          :center="[-122.1754, 37.7683]"
          :zoom="10"
          title="Percent of restaurants with PPP loan"
          variable="pct_with_loan"
          :hideLegend="true"
          staticImage="8.jpg"
        />
        <div class="caption">
          <strong>East Oakland.</strong> Above, percent of restaurants with PPP
          loans. Below, percent of population that is Black or Hispanic. This
          majority Black and hispanic area has low access to PPP resources. Some
          wealthy areas with many restaurants, like the Piedmont Avenue district
          in North Oakland, have a surprisingly low number of PPP loans.
        </div>
        <Map
          classString="w2 h2 below"
          :center="[-122.1754, 37.7683]"
          :zoom="10"
          title="Percent Black and Hispanic"
          variable="black_and_hispanic"
          legendStyle="below"
          staticImage="9.jpg"
        />
      </div>
    </div>

    <div class="w3 l1 p">
      Some of the reason for this divide becomes clearer when looking at the
      individual loans and restaurnts in detail — franchises. For example, one
      company in the wealthy Bay Area suburb of Livermore, QUICKSERVE
      ENTERPRISES INC, recieved a loan for between $5 and 10 million. According
      to their website, QUICKSERVE ENTERPRISES INC "owns and operates fast food
      restaurants," including several Burger King restaurants in the
      less-wealthy Central Valley. While those Burger King establishments count
      as restaurants in the ZIP code where they are located, all of the PPP loan
      money is being counted in Livermore, where their parent corporation is
      located. However, I believe it is defensible and even desireable to
      represent the data this way. With their $5-10 million loan, QUICKSERVE
      ENTERPRISES INC aims to retain 22 employees — likely to be corporate
      employees at the Livermore office.
    </div>
    <div class="w3 l1 p">
      The prevalence of franchised restaurants in low-income areas is part of
      the same pattern of (dis)investment that the PPP is perpetuating. In this
      respect, the PPP both a creator of inequity and also a yardstick by which
      existing inequity can to some extent be measured. These businesses abound
      in the upper-end of the PPP loan dataset: other corporations to recieve
      the maximum possible loan were Great American Chicken Corp. (KFC) and
      Soutern Caliornia Pizza, a private equity-owned Pizza Hut franchisee. In
      the
      <a href="#interactive">interactive map</a> below, you can see the names of
      these large loan recipients.
    </div>

    <Title text="Two broad trends" />

    <div class="w3 l1 p">
      Two broad trends stand out in the map exploration: a correlation with
      income, and a correlation with the percent of the population that is
      non-white, particularly Hispanic. I looked at the correlation of PPP
      outcomes with these factors and found a definite correlation.
    </div>
    <div class="share">
      <div class="w2">
        <div class="w2" style="height: 400px">
          <img src="../assets/income.png" />
        </div>
        <div class="w2 p caption">
          The median income of a ZIP Code area showed a significant (p&lt;1e-6)
          correlation with the percent of restaurants in that ZIP Code that were
          able to recieve PPP loans.
        </div>
      </div>
      <div class="w2">
        <div class="w2" style="height: 400px">
          <img src="../assets/latinx.png" />
        </div>
        <div class="w2 p caption">
          The percent of the ZIP Code that identified as Hispanic also showed a
          significant (p&lt;1e-6) correlation with the percent of restaurants in
          that ZIP Code that were able to recieve PPP loans.
        </div>
      </div>
    </div>

    <div class="w3 l1 p">
      Individually, each of these variables explain about 12%* of the variation
      in local PPP loan access rates. Together, they explain 16%. I examined a
      handful of other socioeconomic variables that might be assumed to be
      related: Black, white, and Asian population breakdowns, age, and
      population density. However, the only other variable that held up in a
      linear multivariate regression was the total population of the ZCTA. This
      effect is much smaller than the effect of median income or percent
      Hispanic.

      <div class="caption float left">
        *this is the same as the r<sup>2</sup>
      </div>
    </div>

    <Title text="Fresno and San Diego" />
    <div class="w4">
      <Map
        classString="w3 h2"
        :center="[-119.79, 36.7378]"
        :zoom="9.5"
        title="Percent of restaurants with PPP loan"
        variable="pct_with_loan"
        legendStyle="right"
        staticImage="10.jpg"
      />
    </div>

    <div class="w4">
      <Map
        classString="w3 h2"
        :center="[-119.79, 36.7378]"
        :zoom="9.5"
        title="% Black and Hispanic"
        variable="black_and_hispanic"
        legendStyle="right"
        staticImage="11.jpg"
      />
    </div>

    <div class="w3 p">
      Not every city perfectly illustrates this broad pattern. For example, the
      Fresno area, despite being highly segregated, shows little obvious
      correlation between demographic trends and PPP loan access.
    </div>

    <div class="w4">
      <Map
        classString="w3 h2"
        :center="[-117.1011, 32.79]"
        :zoom="8.9"
        title="Percent of restaurants with PPP loan"
        variable="loan_per_company"
        legendStyle="right"
        staticImage="12.jpg"
      />
    </div>
    <div class="w4">
      <Map
        classString="w3 h2"
        :center="[-117.1011, 32.79]"
        :zoom="8.9"
        title="% Black and Hispanic"
        variable="black_and_hispanic"
        legendStyle="right"
        staticImage="13.jpg"
      />
    </div>

    <div class="w3 p">
      San Diego however shows many of the same patterns observed in the San
      Francisco and Los Angeles metro areas. Notice that the pattern becomes
      less clear in the rural areas on the east side of the map. Low population
      density (and a correspondingly low number of restaurants) adds noise to
      the data.
    </div>

    <Title text="Explore the data" id="interactive" />

    <div class="w3 p l1">
      In this interactive map, you can further explore the data. Choose from
      variables to map with the selection bar at the bottom. Hover or click on a
      ZIP Code area to see statistics for that location below the map next to
      the legend. You can also find a list of large loan recipients here.
    </div>

    <Map
      classString="w4 h3 below interactive"
      variable="pct_with_loan"
      :interactive="true"
      legendStyle="below"
    />

    <Title text="About" />
    <div class="w4 l0 p">
      This is a personal project by
      <a href="http://subject.space">Logan Williams</a>. If you notice factual
      or data errors, or have a question or comment, please don't hesitate to
      reach out via <a href="mailto:logan.williams@alum.mit.edu">email</a>. Much
      thanks also to my friend
      <a href="https://mookerji.github.io/">Bhaskar Mookerji</a> for discussions
      throughout.
    </div>

    <div class="w4 l0 p">
      Several notes on the data: information on business establishments is from
      the US Census'
      <a
        href="https://www.census.gov/data/developers/data-sets/cbp-nonemp-zbp/zbp-api.html"
        >2017 ZIP Code Business Patterns</a
      >. 2017 is the latest year available. The loan data is from the
      <a
        href="https://home.treasury.gov/policy-issues/cares-act/assistance-for-small-businesses/sba-paycheck-protection-program-loan-level-data"
        >Small Business Administration's release on 6 July 2020</a
      >. Information on specific loan recipients is only available for loans
      greater than $150,000. All loan data is based on applicant submitted
      information and definitely contains errors.
    </div>
  </div>
</template>

<script>
import Title from "../components/Title";
import Map from "../components/Map";

export default {
  name: "Home",
  components: {
    Title,
    Map,
  },
};
</script>
