<img width="300" height="140" alt="jenkins_cicd_whiteboard_diagram" src="https://github.com/user-attachments/assets/d20876a3-dd68-46b1-803d-09dc49c87a67" /># End-to-End CI/CD for Spring Boot with Jenkins, SonarQube, Docker, Kubernetes, and Argo CD

This project demonstrates a complete DevOps CI/CD workflow for a Java Spring Boot application.

The Jenkins pipeline builds the application with Maven, runs SonarQube analysis, builds and pushes a Docker image to Docker Hub, updates the Kubernetes deployment manifest with the new image tag, and lets Argo CD deploy the updated application to Kubernetes.

<svg width="100%" viewBox="0 0 900 420" role="img" style="background: rgb(17, 19, 24); border-radius: 14px;" xmlns="http://www.w3.org/2000/svg">
  <title style="fill:rgb(0, 0, 0);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto">Jenkins E2E CI/CD Pipeline</title>
  <desc style="fill:rgb(0, 0, 0);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto">Whiteboard-style flowchart: Source Code webhook triggers Jenkins, Maven build, SonarQube analysis, Docker image push, image updater updates manifests repo, Argo CD syncs to Kubernetes</desc>

  <defs>
    <marker id="aw" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M1 1L9 5L1 9" fill="none" stroke="#d0d7de" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
    <marker id="ag" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M1 1L9 5L1 9" fill="none" stroke="#3fb950" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
    <marker id="ab" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M1 1L9 5L1 9" fill="none" stroke="#58a6ff" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  <mask id="imagine-text-gaps-ytmbew" maskUnits="userSpaceOnUse"><rect x="0" y="0" width="900" height="420" fill="white"/><rect x="17.742286682128906" y="264.63616943359375" width="80.73339080810547" height="18.772976875305176" fill="black" rx="2"/><rect x="92.63389587402344" y="206.908935546875" width="47.76961898803711" height="15.363828659057617" fill="black" rx="2"/><rect x="156.9995880126953" y="219.49978637695312" width="58.0008430480957" height="19.909358978271484" fill="black" rx="2"/><rect x="276.1463623046875" y="219.49978637695312" width="43.707279205322266" height="19.909358978271484" fill="black" rx="2"/><rect x="344.6248474121094" y="188.908935546875" width="25.80207061767578" height="15.363828659057617" fill="black" rx="2"/><rect x="376.80670166015625" y="221.77255249023438" width="63.15188217163086" height="16.500210762023926" fill="black" rx="2"/><rect x="470.6248474121094" y="188.908935546875" width="25.80207061767578" height="15.363828659057617" fill="black" rx="2"/><rect x="496.1463623046875" y="219.49978637695312" width="43.707279205322266" height="19.909358978271484" fill="black" rx="2"/><rect x="586.8067016601562" y="263.7725524902344" width="63.15188217163086" height="16.500210762023926" fill="black" rx="2"/><rect x="559.0807495117188" y="278.908935546875" width="118.02778625488281" height="15.363810539245605" fill="black" rx="2"/><rect x="574.7237548828125" y="133.77255249023438" width="87.31867218017578" height="16.500210762023926" fill="black" rx="2"/><rect x="687.044921875" y="76.908935546875" width="74.09268951416016" height="15.363828659057617" fill="black" rx="2"/><rect x="695.28369140625" y="88.908935546875" width="57.617008209228516" height="15.363828659057617" fill="black" rx="2"/><rect x="761.6702880859375" y="137.77255249023438" width="63.522953033447266" height="16.500210762023926" fill="black" rx="2"/><rect x="777.9081420898438" y="150.77255249023438" width="32.94340896606445" height="16.500210762023926" fill="black" rx="2"/><rect x="767.7162475585938" y="355.7725524902344" width="52.204833984375" height="16.500210762023926" fill="black" rx="2"/><rect x="645.7882080078125" y="357.7725524902344" width="68.4236068725586" height="16.500210762023926" fill="black" rx="2"/><rect x="60" y="392.908935546875" width="68.60078048706055" height="15.363828659057617" fill="black" rx="2"/><rect x="170" y="392.908935546875" width="96.06021881103516" height="15.363828659057617" fill="black" rx="2"/><rect x="310" y="392.908935546875" width="68.60078430175781" height="15.363828659057617" fill="black" rx="2"/><rect x="772.621337890625" y="392.908935546875" width="101.55210876464844" height="15.363828659057617" fill="black" rx="2"/></mask></defs>

  <!-- ═══ SOURCE CODE icon ═══ -->
  <g style="fill:rgb(0, 0, 0);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto">
    <circle cx="58" cy="230" r="34" fill="none" stroke="#d0d7de" stroke-width="1.8" style="fill:none;stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <!-- git icon -->
    <circle cx="50" cy="222" r="5" fill="#58a6ff" style="fill:rgb(88, 166, 255);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <circle cx="66" cy="222" r="5" fill="#58a6ff" style="fill:rgb(88, 166, 255);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <circle cx="58" cy="240" r="5" fill="#58a6ff" style="fill:rgb(88, 166, 255);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="50" y1="227" x2="58" y2="235" stroke="#58a6ff" stroke-width="1.8" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="66" y1="227" x2="58" y2="235" stroke="#58a6ff" stroke-width="1.8" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <text x="58" y="278" text-anchor="middle" font-family="monospace" font-size="12" fill="#d0d7de" style="fill:rgb(208, 215, 222);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:12px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Source Code</text>
  </g>

  <!-- webhook label + arrow -->
  <text x="117" y="218" text-anchor="middle" font-family="monospace" font-size="10" fill="#8b949e" style="fill:rgb(139, 148, 158);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Webhook</text>
  <line x1="92" y1="230" x2="142" y2="230" stroke="#d0d7de" stroke-width="1.5" marker-end="url(#aw)" style="fill:rgb(0, 0, 0);stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.5px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ JENKINS ═══ -->
  <rect x="144" y="208" width="84" height="44" rx="22" fill="none" stroke="#d0d7de" stroke-width="1.8" style="fill:none;stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="186" y="234" text-anchor="middle" font-family="monospace" font-size="13" fill="#d0d7de" style="fill:rgb(208, 215, 222);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:13px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Jenkins</text>

  <!-- arrow -->
  <line x1="228" y1="230" x2="256" y2="230" stroke="#d0d7de" stroke-width="1.5" marker-end="url(#aw)" style="fill:rgb(0, 0, 0);stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.5px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ MAVEN ═══ -->
  <rect x="258" y="208" width="80" height="44" rx="22" fill="none" stroke="#d0d7de" stroke-width="1.8" style="fill:none;stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="298" y="234" text-anchor="middle" font-family="monospace" font-size="13" fill="#d0d7de" style="fill:rgb(208, 215, 222);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:13px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Maven</text>

  <!-- Yes bubble above -->
  <ellipse cx="358" cy="196" rx="17" ry="12" fill="none" stroke="#d0d7de" stroke-width="1.4" style="fill:none;stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="358" y="200" text-anchor="middle" font-family="monospace" font-size="10" fill="#d0d7de" style="fill:rgb(208, 215, 222);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Yes</text>

  <!-- arrow -->
  <line x1="338" y1="230" x2="366" y2="230" stroke="#d0d7de" stroke-width="1.5" marker-end="url(#aw)" style="fill:rgb(0, 0, 0);stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.5px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ SONARQUBE (diamond) ═══ -->
  <polygon points="408,208 452,230 408,252 364,230" fill="none" stroke="#d0d7de" stroke-width="1.8" style="fill:none;stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="408" y="234" text-anchor="middle" font-family="monospace" font-size="11" fill="#d0d7de" style="fill:rgb(208, 215, 222);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:11px;font-weight:400;text-anchor:middle;dominant-baseline:auto">SonarQube</text>

  <!-- Yes bubble above -->
  <ellipse cx="484" cy="196" rx="17" ry="12" fill="none" stroke="#d0d7de" stroke-width="1.4" style="fill:none;stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="484" y="200" text-anchor="middle" font-family="monospace" font-size="10" fill="#d0d7de" style="fill:rgb(208, 215, 222);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Yes</text>

  <!-- arrow -->
  <line x1="452" y1="230" x2="480" y2="230" stroke="#d0d7de" stroke-width="1.5" marker-end="url(#aw)" style="fill:rgb(0, 0, 0);stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.5px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ TESTS ═══ -->
  <rect x="482" y="208" width="72" height="44" rx="22" fill="none" stroke="#d0d7de" stroke-width="1.8" style="fill:none;stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="518" y="234" text-anchor="middle" font-family="monospace" font-size="13" fill="#d0d7de" style="fill:rgb(208, 215, 222);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:13px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Tests</text>

  <!-- arrow to docker -->
  <line x1="554" y1="230" x2="580" y2="230" stroke="#3fb950" stroke-width="1.8" marker-end="url(#ag)" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ DOCKER (circle with whale) ═══ -->
  <g style="fill:rgb(0, 0, 0);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto">
    <circle cx="618" cy="230" r="34" fill="none" stroke="#3fb950" stroke-width="1.8" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <!-- whale body -->
    <rect x="604" y="222" width="28" height="16" rx="4" fill="none" stroke="#3fb950" stroke-width="1.4" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <!-- whale top containers -->
    <rect x="607" y="216" width="7" height="7" rx="1.5" fill="none" stroke="#3fb950" stroke-width="1.2" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <rect x="617" y="213" width="7" height="10" rx="1.5" fill="none" stroke="#3fb950" stroke-width="1.2" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <!-- tail -->
    <path d="M632 230 Q638 222 634 218" fill="none" stroke="#3fb950" stroke-width="1.2" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <text x="618" y="276" text-anchor="middle" font-family="monospace" font-size="11" fill="#3fb950" style="fill:rgb(63, 185, 80);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:11px;font-weight:400;text-anchor:middle;dominant-baseline:auto">New Image</text>
    <text x="618" y="290" text-anchor="middle" font-family="monospace" font-size="10" fill="#8b949e" style="fill:rgb(139, 148, 158);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:middle;dominant-baseline:auto">pushed to Docker Hub</text>
  </g>

  <!-- Docker → up to Image Updater -->
  <line x1="618" y1="196" x2="618" y2="136" stroke="#3fb950" stroke-width="1.5" marker-end="url(#ag)" mask="url(#imagine-text-gaps-ytmbew)" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.5px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ IMAGE UPDATER (circle) ═══ -->
  <g style="fill:rgb(0, 0, 0);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto">
    <circle cx="618" cy="108" r="26" fill="none" stroke="#58a6ff" stroke-width="1.8" style="fill:none;stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <!-- person icon simplified -->
    <circle cx="618" cy="100" r="7" fill="none" stroke="#58a6ff" stroke-width="1.4" style="fill:none;stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <path d="M606 118 Q618 110 630 118" fill="none" stroke="#58a6ff" stroke-width="1.4" style="fill:none;stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <text x="618" y="146" text-anchor="middle" font-family="monospace" font-size="11" fill="#58a6ff" style="fill:rgb(88, 166, 255);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:11px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Image updater</text>
  </g>

  <!-- "Updates with new Image" label -->
  <text x="724" y="88" text-anchor="middle" font-family="monospace" font-size="10" fill="#8b949e" style="fill:rgb(139, 148, 158);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Updates with</text>
  <text x="724" y="100" text-anchor="middle" font-family="monospace" font-size="10" fill="#8b949e" style="fill:rgb(139, 148, 158);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:middle;dominant-baseline:auto">new Image</text>

  <!-- Image Updater → Manifests Repo -->
  <line x1="644" y1="108" x2="760" y2="108" stroke="#58a6ff" stroke-width="1.5" marker-end="url(#ab)" style="fill:rgb(0, 0, 0);stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.5px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ MANIFESTS REPO (rounded rect with git icon) ═══ -->
  <g style="fill:rgb(0, 0, 0);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto">
    <circle cx="794" cy="108" r="30" fill="none" stroke="#58a6ff" stroke-width="1.8" style="fill:none;stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <!-- git-like icon -->
    <rect x="782" y="98" width="24" height="20" rx="3" fill="none" stroke="#58a6ff" stroke-width="1.2" style="fill:none;stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="788" y1="104" x2="800" y2="104" stroke="#58a6ff" stroke-width="1.2" style="fill:rgb(0, 0, 0);stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="788" y1="110" x2="800" y2="110" stroke="#58a6ff" stroke-width="1.2" style="fill:rgb(0, 0, 0);stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="788" y1="116" x2="796" y2="116" stroke="#58a6ff" stroke-width="1.2" style="fill:rgb(0, 0, 0);stroke:rgb(88, 166, 255);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <text x="794" y="150" text-anchor="middle" font-family="monospace" font-size="11" fill="#58a6ff" style="fill:rgb(88, 166, 255);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:11px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Manifests</text>
    <text x="794" y="163" text-anchor="middle" font-family="monospace" font-size="11" fill="#58a6ff" style="fill:rgb(88, 166, 255);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:11px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Repo</text>
  </g>

  <!-- Manifests Repo ↓ to Argo CD (vertical dashed) -->
  <line x1="794" y1="138" x2="794" y2="298" stroke="#8b949e" stroke-width="1.2" stroke-dasharray="5 4" mask="url(#imagine-text-gaps-ytmbew)" style="fill:rgb(0, 0, 0);stroke:rgb(139, 148, 158);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-dasharray:5px, 4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ ARGO CD (circle) ═══ -->
  <g style="fill:rgb(0, 0, 0);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto">
    <circle cx="794" cy="328" r="28" fill="none" stroke="#3fb950" stroke-width="1.8" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <circle cx="794" cy="320" r="7" fill="none" stroke="#3fb950" stroke-width="1.4" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <path d="M782 338 Q794 330 806 338" fill="none" stroke="#3fb950" stroke-width="1.4" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <text x="794" y="368" text-anchor="middle" font-family="monospace" font-size="11" fill="#3fb950" style="fill:rgb(63, 185, 80);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:11px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Argo CD</text>
  </g>

  <!-- Argo CD → Kubernetes -->
  <line x1="766" y1="328" x2="714" y2="328" stroke="#3fb950" stroke-width="1.8" marker-end="url(#ag)" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>

  <!-- ═══ KUBERNETES (circle with wheel) ═══ -->
  <g style="fill:rgb(0, 0, 0);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto">
    <circle cx="680" cy="328" r="30" fill="none" stroke="#3fb950" stroke-width="1.8" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.8px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <!-- k8s helm wheel -->
    <circle cx="680" cy="328" r="9" fill="none" stroke="#3fb950" stroke-width="1.4" style="fill:none;stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="680" y1="313" x2="680" y2="319" stroke="#3fb950" stroke-width="1.4" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="680" y1="337" x2="680" y2="343" stroke="#3fb950" stroke-width="1.4" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="665" y1="328" x2="671" y2="328" stroke="#3fb950" stroke-width="1.4" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="689" y1="328" x2="695" y2="328" stroke="#3fb950" stroke-width="1.4" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="669" y1="317" x2="673" y2="323" stroke="#3fb950" stroke-width="1.4" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="687" y1="333" x2="691" y2="339" stroke="#3fb950" stroke-width="1.4" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="691" y1="317" x2="687" y2="323" stroke="#3fb950" stroke-width="1.4" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <line x1="673" y1="333" x2="669" y2="339" stroke="#3fb950" stroke-width="1.4" stroke-linecap="round" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.4px;stroke-linecap:round;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
    <text x="680" y="370" text-anchor="middle" font-family="monospace" font-size="11" fill="#3fb950" style="fill:rgb(63, 185, 80);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:11px;font-weight:400;text-anchor:middle;dominant-baseline:auto">Kubernetes</text>
  </g>

  <!-- ── Legend bottom left ── -->
  <line x1="30" y1="400" x2="58" y2="400" stroke="#d0d7de" stroke-width="1.5" style="fill:rgb(0, 0, 0);stroke:rgb(208, 215, 222);color:rgb(0, 0, 0);stroke-width:1.5px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="64" y="404" font-family="monospace" font-size="10" fill="#8b949e" style="fill:rgb(139, 148, 158);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:start;dominant-baseline:auto">CI pipeline</text>
  <line x1="140" y1="400" x2="168" y2="400" stroke="#3fb950" stroke-width="1.5" style="fill:rgb(0, 0, 0);stroke:rgb(63, 185, 80);color:rgb(0, 0, 0);stroke-width:1.5px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="174" y="404" font-family="monospace" font-size="10" fill="#8b949e" style="fill:rgb(139, 148, 158);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:start;dominant-baseline:auto">success / deploy</text>
  <line x1="280" y1="400" x2="308" y2="400" stroke="#8b949e" stroke-width="1.2" stroke-dasharray="4 3" style="fill:rgb(0, 0, 0);stroke:rgb(139, 148, 158);color:rgb(0, 0, 0);stroke-width:1.2px;stroke-dasharray:4px, 3px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:&quot;Anthropic Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, sans-serif;font-size:16px;font-weight:400;text-anchor:start;dominant-baseline:auto"/>
  <text x="314" y="404" font-family="monospace" font-size="10" fill="#8b949e" style="fill:rgb(139, 148, 158);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:start;dominant-baseline:auto">GitOps sync</text>

  <!-- watermark -->
  <text x="870" y="404" text-anchor="end" font-family="monospace" font-size="10" fill="#21262d" style="fill:rgb(33, 38, 45);stroke:none;color:rgb(0, 0, 0);stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;opacity:1;font-family:monospace;font-size:10px;font-weight:400;text-anchor:end;dominant-baseline:auto">Jenkins E2E CI/CD</text>
</svg>



## Project Structure

```text
java-maven-sonar-argocd-helm-k8s/
+-- Argo CD/
|   +-- argocd-basic.yaml
+-- spring-boot-app/
|   +-- Dockerfile
|   +-- JenkinsAgent.Dockerfile
|   +-- JenkinsFile
|   +-- pom.xml
|   +-- src/
+-- spring-boot-app-manifests/
    +-- deployment.yml
    +-- service.yml
```

## Tools Used

- **Jenkins**: CI/CD automation
- **Maven**: Java build and test tool
- **SonarQube**: Static code analysis
- **Docker**: Container image build and push
- **Docker Hub**: Container registry
- **Kubernetes**: Application runtime platform
- **Argo CD**: GitOps deployment controller

## Current Images

Application image:

```text
naveenreddy9/spring-boot-app:<BUILD_NUMBER>
```

Jenkins agent image:

```text
naveenreddy9/maven-docker-java17-agent:v1
```

The custom Jenkins agent image includes:

- Java 17
- Maven 3.9.9
- Docker CLI 29.5.3

This avoids repeated installation of Java and Docker CLI during every pipeline run.

## Jenkins Pipeline Flow

The pipeline is defined in:

```text
spring-boot-app/JenkinsFile
```

Pipeline stages:

1. **Checkout**
   - Clones the GitHub repository from the `main` branch.

2. **Build and Test**
   - Runs Maven package:

   ```bash
   mvn clean package
   ```

3. **Static Code Analysis**
   - Runs SonarQube analysis with the pinned Maven Sonar plugin:

   ```text
   org.sonarsource.scanner.maven:sonar-maven-plugin:4.0.0.4121
   ```

4. **Build and Push Docker Image**
   - Builds the Spring Boot Docker image.
   - Tags it with the Jenkins build number.
   - Pushes it to Docker Hub.

5. **Update Deployment File**
   - Updates `spring-boot-app-manifests/deployment.yml`.
   - Replaces the image tag with the latest Jenkins build number.
   - Commits and pushes the updated manifest back to GitHub.

6. **Post Actions**
   - Fixes workspace ownership after the Docker-based Jenkins agent exits.

## Jenkins Credentials

Create these credentials in Jenkins under:

```text
Manage Jenkins -> Credentials -> System -> Global credentials
```

### SonarQube Token

```text
ID: sonarqube
Kind: Secret text
Secret: SonarQube user token
```

Used by:

```groovy
withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')])
```

### Docker Hub Credentials

```text
ID: docker-cred
Kind: Username with password
Username: naveenreddy9
Password: Docker Hub password or access token
```

Used by:

```groovy
docker.withRegistry('https://index.docker.io/v1/', "docker-cred")
```

### GitHub Token

```text
ID: github
Kind: Secret text
Secret: GitHub personal access token
```

The token must have permission to push to:

```text
VenkataNaveenReddyYaparla/Jenkins_E2E_CICD
```

For a classic GitHub token, enable the `repo` permission.

## Custom Jenkins Agent Image

The Jenkins pipeline uses this custom agent:

```text
naveenreddy9/maven-docker-java17-agent:v1
```

Its Dockerfile is:

```text
spring-boot-app/JenkinsAgent.Dockerfile
```

Build the agent image:

```bash
cd spring-boot-app
docker build -f JenkinsAgent.Dockerfile -t naveenreddy9/maven-docker-java17-agent:v1 .
```

Push the agent image:

```bash
docker login
docker push naveenreddy9/maven-docker-java17-agent:v1
```

Verify the image:

```bash
docker run --rm naveenreddy9/maven-docker-java17-agent:v1 sh -lc "java -version && mvn -v && docker --version"
```

## Run the Application Locally

### Using Maven

```bash
cd spring-boot-app
mvn clean package
java -jar target/spring-boot-web.jar
```

Open:

```text
http://localhost:8080
```

### Using Docker

```bash
cd spring-boot-app
docker build -t spring-boot-app .
docker run --rm -p 8080:8080 spring-boot-app
```

Open:

```text
http://localhost:8080
```

## Kubernetes Deployment

Kubernetes manifests are stored in:

```text
spring-boot-app-manifests/
```

Deployment:

```text
spring-boot-app-manifests/deployment.yml
```

Service:

```text
spring-boot-app-manifests/service.yml
```

The deployment uses the Docker image:

```text
naveenreddy9/spring-boot-app:<BUILD_NUMBER>
```

The service is a `NodePort` service that exposes the app on port `80` and forwards traffic to container port `8080`.

Apply manually:

```bash
kubectl apply -f spring-boot-app-manifests/deployment.yml
kubectl apply -f spring-boot-app-manifests/service.yml
```

Check resources:

```bash
kubectl get pods
kubectl get svc
```

For local testing:

```bash
kubectl port-forward svc/spring-boot-app-service 8080:80
```

Open:

```text
http://localhost:8080
```

## Argo CD Setup

Argo CD should track the Git repository that contains:

```text
spring-boot-app-manifests/
```

When Jenkins pushes a new image tag to `deployment.yml`, Argo CD detects the Git change and syncs the new deployment to Kubernetes.

Example Argo CD application manifest:

```text
Argo CD/argocd-basic.yaml
```

Apply it:

```bash
kubectl apply -f "Argo CD/argocd-basic.yaml"
```

Check Argo CD application status:

```bash
kubectl get applications -n argocd
```

## SonarQube Notes

The SonarQube scanner must run with Java 17.

The custom Jenkins agent image already includes Java 17. This fixes errors like:

```text
UnsupportedClassVersionError
class file version 61.0
this version of the Java Runtime only recognizes class file versions up to 55.0
```

Meaning:

```text
class file version 61 = Java 17
class file version 55 = Java 11
```

## Common Issues and Fixes

### `ERROR: Could not find credentials entry with ID 'github'`

Create a Jenkins credential:

```text
ID: github
Kind: Secret text
```

Use a GitHub token with push permission.

### `ERROR: docker-cred`

Create Docker Hub credentials in Jenkins:

```text
ID: docker-cred
Kind: Username with password
```

Make sure it is under **System -> Global credentials**.

### `client version 1.41 is too old`

The Docker CLI inside the Jenkins agent is too old for the Docker daemon.

Fix: use the custom Jenkins agent image:

```text
naveenreddy9/maven-docker-java17-agent:v1
```

### `fatal: not in a git directory`

The workspace is not a real Git checkout.

Fix: make sure the Jenkins `Checkout` stage performs a real Git checkout:

```groovy
git branch: 'main', url: 'https://github.com/VenkataNaveenReddyYaparla/Jenkins_E2E_CICD.git'
```

### `detected dubious ownership`

Add the Jenkins workspace as a safe Git directory:

```bash
git config --global --add safe.directory "$WORKSPACE"
```

This is already handled in the current Jenkinsfile.

## Useful Commands

Check Docker image:

```bash
docker images | grep spring-boot-app
```

Check pushed image:

```bash
docker pull naveenreddy9/spring-boot-app:<BUILD_NUMBER>
```

Check Kubernetes rollout:

```bash
kubectl rollout status deployment/spring-boot-app
```

Check pod logs:

```bash
kubectl logs -l app=spring-boot-app
```

Check Argo CD apps:

```bash
kubectl get applications -n argocd
```

## Final Workflow

1. Developer pushes code to GitHub.
2. Jenkins pipeline starts.
3. Jenkins builds the Spring Boot application.
4. Jenkins runs SonarQube analysis.
5. Jenkins builds and pushes Docker image.
6. Jenkins updates `deployment.yml` with the new image tag.
7. Jenkins commits and pushes the manifest change.
8. Argo CD detects the Git change.
9. Argo CD syncs the new version to Kubernetes.
10. Kubernetes runs the updated Spring Boot application.
