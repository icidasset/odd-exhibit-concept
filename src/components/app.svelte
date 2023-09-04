<script>
  import * as odd from "@oddjs/odd";
  import BareTree from "@oddjs/odd/fs/bare/tree";

  // CONSTANTS

  /**
   * Name of the published Fission app.
   */
  const APP_NAME = "exhibit-concept";
  const APP_OWNER = "icidasset";

  /**
   * Token generated using Fission CLI.
   *
   * ```
   * fission app delegate --app-name exhibit-concept --did did:key:zStEZpzSMtTt9k2vszgvCwF4fLQQSyA15W5AQ4z3AR6Bx4eFJ5crJFbuGxKmbma4 --lifetime 3153600000
   * ```
   * This one is valid for a 100 years.
   */
  const APPEND_TOKEN =
    "eyJ1YXYiOiIxLjAuMCIsImFsZyI6IkVkRFNBIiwiY3R5IjpudWxsLCJ0eXAiOiJKV1QifQ.eyJuYmYiOjE2OTM4MzQxMzQsImlzcyI6ImRpZDprZXk6ejZNa25FV1E0OUdzMjdwRk53dHBaWmljMmkzekNpcFU0VGdEbjZxS3RqWm9DQ1Y1IiwicHJmIjoiZXlKaGJHY2lPaUpTVXpJMU5pSXNJblI1Y0NJNklrcFhWQ0lzSW5WaGRpSTZJakV1TUM0d0luMC5leUpoZFdRaU9pSmthV1E2YTJWNU9ubzJUV3R1UlZkUk5EbEhjekkzY0VaT2QzUndXbHBwWXpKcE0zcERhWEJWTkZSblJHNDJjVXQwYWxwdlEwTldOU0lzSW1WNGNDSTZNekkzT0RBMk5USTJPVEFzSW1aamRDSTZXMTBzSW1semN5STZJbVJwWkRwclpYazZlakV6VmpOVGIyY3lXV0ZWUzJoa1IwTnRaM2c1VlZwMVZ6RnZNVk5vUmtwWll6WkVka2RaWlRkT1ZIUTJPRGxPYjB3ek9FWTNOM2RYYld0dVlYaG9jbVJsY0RaWlMwNU5VemRDZEhSQ1ZuWTNhbE5vZVRkTWEzRlNVR0ZuTm5SWGNVRjZhbWhsTkROVlZ6WmpkamxHT1dsTGVFY3lWVXA1YW5sRmVHbHFZamxWVkhvNWQydHhXSFpoVG1sbFlVTnBjakpYWkhodWNVTnhlSFp2YzBSeE5WRkxkMDUzUTNkdk5HVklOVXBYWmxOemRWVklPRFpqVGxZMk4xTkJSWG8yUlZGdU9UUjJSbTVqVEVSRk1tSllSbUpaY0hsaWVGbzRjMUV5ZURoWVVIcE1UbkJHV21GeVRrSmpZM050UTB3MldXUkJaV2xPWWpkR1JsZFZUVTF2ZFRWalVqbDNVRkZtV1hCSVkyMU1aMlJDVUhCQ01URTVWemhtT1c5UVJXMUdXV2xRUlhWUllsaFpXWHBMY21ZeVpHUlNkbVJXYTNCa1MwSTBaa1paYzNWWmFVUmpVemswVEhwcWJYZHhVVEY0VG5WMmRFZDBjMFZ5Y1dkVlZVVjJSM3BqZFZKT1VrWjRWVkJ0VmtGUU9FUndhMHhNZDNGTmNXRkhha1I0WVhWMmVrWmpXRmh6ZERGR1ozRXhhbFpPYURaNU0yMVJVazRpTENKdVltWWlPakUyTnpZM05UWXlOVGtzSW5CeVppSTZJbVY1U21oaVIyTnBUMmxLVTFWNlNURk9hVWx6U1c1U05XTkRTVFpKYTNCWVZrTkpjMGx1Vm1oa2FVazJTV3BGZFUxRE5IZEpiakF1WlhsS2FHUlhVV2xQYVVwcllWZFJObUV5VmpWUGJtOTRUVEZaZWxVeU9XNU5iR3hvVmxWMGIxcEZaRVJpVjJRMFQxWldZV1JXWTNoaWVrWlVZVVZhUzFkWFRUSlNTRnBJVjFkVk0xUnNVakJPYW1jMVZHMDVUVTE2YUVkT2VtUXpWakl4Y21KdFJqUmhTRXByV2xoQk1sZFZkRTlVVmswelVXNVNNRkZzV2pKT01uQlVZVWhyTTFSSGRIaFZiRUpvV25wYU1GWXpSa0psYlhCdldsUlJlbFpXWXpKWk0xazFVbXBzY0ZNemFFaE5iRlpMWlZkd05WSllhSEJoYlVrMVZsWlNOazlZWkhKalZtZ3lXVlUxY0ZwWFJrUmhXRWw1VmpKU05HSnVSa1JqV0dneVlqTk9SV05VVmxKVE0yUlBaREJPTTJKNlVteFRSRlpMVmpKYVZHTXpWbFpUUkdjeVdUQTFWMDVxWkZSUlZWWTJUbXRXVW1KcWF6QmthMXAxV1RCNFJWSlVTbWxYUlZwcFYxaENOVmx1YUdGUFNFNVNUVzVuTkZkR1FqWlVSVFYzVW14d2FHTnJOVU5aTWs1NllsVk9UVTVzYkd0UlYxWndWRzFKTTFKcldsaFdWVEZPWWpOVk1Wa3hTVFZrTVVKU1dteHNkMU5IVG5SVVIyUnJVV3hDZDFGcVJYaFBWbU0wV21wc2RsVkZWblJTYkd4d1ZVVldNVlZYU2xsWFZtdzJVek5LYlUxdFVtdFZibHByVm0xMGQxcEZkRU5PUjFwSFYxaE9NVmRYYkVWWk1VMDFUa1Y0Tm1GdE1UTmpWa1Y0WlVVMU1XUnVVa2hrU0U1R1kyNUdibFpXVmtaa2EyUTJXVE5XVTFSc1NrZGxSbFpSWWxaYVFsVkVhRVZqUjNSTlZFaGtlRlJZUm1oU01uQkZaVWRHTVdSdWNFZFpNV2haWXpOUmVGSnRaSGhOVjNCWFZHMW5NbVZVVG5SVlZrcFBTV2wzYVZwWWFIZEphbTk2VFdwak5FMUVXVEZOYWxrMVRVTjNhVnB0VGpCSmFuQmlXRk4zYVdGWVRucEphbTlwV2tkc2EwOXRkR3hsVkhBMlRWUk9WMDB4VG5aYWVrcGFXVlpXVEdGSFVraFJNakZ1WlVSc1ZsZHVWbGhOVnpoNFZUSm9SMU5zYkdwT2ExSXlVakZzYkU0d05WVmtSRmswVDFVMWRsUkVTbGRVYmtaMlZHcEtlV016Y0VoalIxcFRZVmRvZDJKc2EzaFNWbFp4VmpCT2JVMHdNVWRsUmswd1lsaGFNMWt3UmxWVlZWSjRVM3BPUjFFeGNFMWlWM1JDWWxSb1dsbFVUa0phUlZwR1dsUmFSRkp0VmxGWk0wbzFVa1ZHZEZVd1NqTk9WbEl5Vmtoa2NGbFdTa2RWZWs1NlQxaFNkV05WZHpWV1J6UXdWakJ3TW1WV1JuZGFWekZPWWpKYU0wMXVaR2xXVmtaWVRURnJlVkZXUW5CalZscHdWRmRrTTFGdFRubFdia1o1Vkd4bmVWZHFhRWRSVjFwcVYydGtTMU5IY0RWYWF6RjNWV3BPYjA1SWFHbGtTRm93WlZaYWNrOVZaRmRhTTA1UFltdDBOVnBJV1RSak1FWkpWRlJzVmxSc2JEQlZSbXgwVlZaR2VWbFVaRTFQVjNCc1lUSm9hMUpzVWxkV1JrNVBWR3BrUWs1dFNreFpNRTVHV1c1d01WWnJaRFJrYkdkNVYxaGtXRTVHYUcxV2JXeFJXakIwY0dKWGRIbE9iRTV0VjFSc2NFMVhUbFJWYmtwUlRtdFpNazR6UVRGVk1sVTFUVEpHVEdGWFdURmpiRkV5WWxWNGVHUkdXazlYUmtwTVYwZG9NMDVYZEV0VlZsa3lVVmhGTlZGdGNIUmxiVTUyVkVSc1QySXlVWHBVUjFadVlXc3hZVnBXWkV4a2JXZDVWa2hzV2xkRVpHcFdhbFphVlRKYVFtTjZTbXBhYm1nelQxaE9WMUpXVm5KaWJGWTJaR3BvV21WVVJsVk9ha3BTWWxkU1UwNVlaR2hXYW1oSlpHbEpjMGx0TldsYWFVazJUVlJaTTA1cVdURk5hbGw2VFVOM2FXTklTbTFKYW5CMVpGZDRjMHhEU25ka1IwMXBUMmxLVkZaV1FrWlZiRGxXVlRCV1UwbHBkMmxqYms1cVNXcHZhVXRwU2prdVkyZE1NMVpCUTBwaE5GWXdjMHd4TXpSbGNrcENVRFZoWTBRM1dVRk5TMHRDYWxCek5uVkROVUZIVG14T2RIQkZOR04yWjAxVlRISlhVbU5XVFhweU9ITlNUVmR5VlRGbk5XcHhPRGxwWlZWcFJHRTNkakpxUmtsMVRXczRWVjlPUW1zM2NUZEZVemhpZVVSbmRrbENORWsyUjJoMWJYbzBOM1pRYlVoTlkzWldXbEUzWWtKa1dXaEhjMWx5UldkRVpHRmpObUZQVkdsbmFGSmlWbU5ETTJWYU5sRklSVzF0VjFWemJGcE5jMlJxTlVSbU4zTjNUVGhLVWpCMFpGZzRRa2gyTURNeGMwNHlhbDlhUm1sMFFtSnlNalZoVkdJMmFsRlJXSEUxYUZCV05qRnBlVEF3VVRKd1VFTlNVM053TlZkek9VNUxRMGt0T1U1TGRrOHlPV05MYVdwUFZqWjJhamhQVlZFM1IwMW5ZbTlxY1VocVFYZFFNbVp5WTFkNVRsUmtkVlJtYUhoSlYzTkxhM1ZUTWxCb1gxUk9iVU5pU0Voak4yTlRWVVJUUWtWdWRrOVVTWFZhYkRaMVFVRmhOVzlpVFhkUklpd2ljSFJqSWpvaVUxVlFSVkpmVlZORlVpSXNJbkp6WXlJNklpb2lmUS5XTnBjMWJ0MnU5UVFMYXlsQTFUR1lOd0dYZE9DSmlPaTI3aDMtbkFjTmJjdWI5Y21qOUNUNV9CdU9pNjNfTU05c01mV2xlLXozRHE2ZnhmbksxRDB2THcwaWVPLTJ3cmNaOWNUMm5MR2t6cERwVkc2d0xnVklhT3NLMEZzRHJxd290OU12akFuZG5NMTBIRFRPaWVkOENlUXp0QVYzdHRwbWNfaWllMEVFbG80VU5zTTZqQUlab2hZcjVIWG1aZGs2Y1pEdUVkZHdGc3hfS1h3dHBOZHR1N0cwdGxfNG5hWFcwRllzaHNKNmhDSnh4N0JUNms2OXQxRXFPMlBvZTZFZWx2UTF1MHZtQ1ludjBDTGZ2dHAwV3JfNkxUVTFNWW9QOWhwQWpJSkNkVG5XY0J2cndMSjNjNEtsUmxxZlZ4c01ocjRKMkFkR0ZRc0tXOHJWOXdsNWciLCJhdWQiOiJkaWQ6a2V5Ono2TWtnWUdGM3RobjhrMUZ2NHA0ZFdYS3RzWENuTEg3cTl5dzRRZ05QVUxEbURLQiIsImZjdCI6W10sInB0YyI6IkFQUEVORCIsInJzYyI6eyJhcHAiOiJleGhpYml0LWNvbmNlcHQuZmlzc2lvbi5hcHAifSwiZXhwIjo0ODQ3NDM0MTY0fQ.akWyV1Aw6_OUY13jIhhWyTZAA6uK1Cdc2xzy4lWj-tNKXjVD5csWmVSRDeENF0CC1EOJPNI6jRJqsEmgn1X1Aw";

  // STATE

  let program;
  let session;
  let fs;

  let shareDetails = null;
  let usernameToShareWith = APP_OWNER;
  let entries = [];

  //////////
  // INIT //
  //////////
  async function init() {
    program = await odd.program({
      namespace: APP_NAME,
    });

    // Ensure session
    session = program.session;

    if (!session) {
      const { success } = await program.auth.register({
        username: `odd-shared-example-${Date.now()}`,
      });
      if (!success) return alert("Failed to register user");
      session = await program.auth.session();
    }

    // File system things
    fs = session.fs;

    await fs.write(
      odd.path.file("private", "file"),
      new TextEncoder().encode("㊙️")
    );

    // Allow things to be shared to this device
    if ((await program.fileSystem.hasPublicExchangeKey(fs)) === false) {
      program.fileSystem.addPublicExchangeKey(fs);
    }

    // Publish file system changes
    await fs.publish();

    // Append-endpoint things
    entries = await listEntries();
  }

  ///////////
  // SHARE //
  ///////////
  async function share() {
    shareDetails = await fs.sharePrivate([odd.path.file("private", "file")], {
      shareWith: usernameToShareWith,
    });
  }

  ////////
  // 🛠️ //
  ////////

  /**
   * Adds a username and a share id to the "append-endpoint list".
   *
   * @param username {string}
   * @param shareId {number}
   * @returns {Promise<string>} CID
   */
  async function addEntry(username, shareId) {
    const jwt = APPEND_TOKEN;

    return fetch(
      `https://runfission.com/v2/api/append/${APP_NAME}/${username}__${shareId}`,
      {
        method: "PUT",
        body: new Blob(
          [
            JSON.stringify({
              username,
              shareId,
            }),
          ],
          { type: "text/plain" }
        ),
        headers: {
          authorization: `Bearer ${jwt}`,
          contentType: "application/octet-stream",
        },
      }
    ).then((r) => r.text());
  }

  /**
   * @returns {Promise<{ username: string, shareId: number | null }[]>} Usernames and their share ids that were added to "append-endpoint list"
   */
  async function listEntries() {
    const { depot, reference } = program.components;

    const cid = await reference.dns.lookupDnsLink(`${APP_NAME}.fission.app`);
    if (!cid) return;

    const bareTree = await BareTree.fromCID(depot, cid);
    let links;

    try {
      links = await bareTree.ls(["uploads"]);
    } catch (err) {
      return [];
    }

    return Object.entries(links).reduce(async (acc, [k, v]) => {
      const list = await acc;
      const content = await depot.getBlock(v.cid);

      // TODO: Ideally we prefer this method, but it doesn't matter that much
      console.log(new TextDecoder().decode(content));

      const split = k.split("__");

      const [username, shareId] = [
        split.slice(0, split.length - 1).join("__"),
        split.length === 1 ? null : split[split.length - 1],
      ];

      if (!shareId) return list;
      return [...list, { username: k, shareId: parseInt(shareId, 10) }];
    }, Promise.resolve([]));
  }

  // Fin
  let promise = init();
</script>

<div>
  {#await promise}
    <p>Waiting ...</p>
  {:then}
    <p>
      Our username: {session.username}<br />
    </p>
    <p>
      {#if shareDetails}
        Shared with {APP_OWNER} ✅ ID: {shareDetails.shareId}
      {:else}
        <input
          bind:value={usernameToShareWith}
          placeholder="Username to share with"
        />
        <button on:click={share}>Share</button>
      {/if}
    </p>
    <p>
      Entries added to the append-endpoint list:<br />
    </p>
    <ul>
      {#each entries as entry}
        <li>{entry.username} - Share id: {entry.shareId}</li>
      {/each}
    </ul>
    <p>
      {#if shareDetails}
        Add our username & share id to the list: <button
          on:click={async () => {
            await addEntry(session.username, shareDetails.shareId);
            entries = await listEntries();
          }}>Add</button
        >
      {:else}
        Add our username & share id to the list: <button disabled>Add</button>
        <br />
        <em>Share the file first, then you can add it.</em>
      {/if}
    </p>
  {:catch error}
    <p style="color: red">{error}</p>
  {/await}
</div>
