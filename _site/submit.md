<div id="tripetto-1wg3b90"></div>
<script src="https://cdn.jsdelivr.net/npm/@tripetto/runner"></script>
<script src="https://cdn.jsdelivr.net/npm/@tripetto/runner-classic"></script>
<script src="https://cdn.jsdelivr.net/npm/@tripetto/studio"></script>
<script>
    const TRIGGER_TEXT = "It's a wrap!";
    //const TRIGGER_TEXT = "Submit your text-to-image creations of Málaga";

    function replaceFormWithThankYou() {
        // Find the form container
        const formContainer = document.getElementById('tripetto-1wg3b90');

        // Verify container exists
        if (!formContainer) {
            console.error('Form container not found');
            return;
        }

        // Clear the current form container
        formContainer.innerHTML = '';

        // Create a thank you message
        const thankYouDiv = document.createElement('div');
        thankYouDiv.innerHTML = `
            <div>
                <h2>Thank You!</h2>
                <p>Your submission for the Malaga in the Age of AI contest has been successfully received.</p>
                <p>Winners will be announced on Dec 22nd. Good luck!</p>
            </div>
        `;

        // Append the thank you message
        formContainer.appendChild(thankYouDiv);

        // Optional: Log the substitution
        console.log('Form replaced with thank you message');
    }

    document.addEventListener('DOMContentLoaded', function() {
        // Debugging function
        function debugLog(message) {
            console.log(`[Tripetto Debug] ${message}`);
        }

        // Check if required Tripetto libraries are loaded
        debugLog(`TripettoStudio exists: ${!!window.TripettoStudio}`);
        debugLog(`TripettoClassic exists: ${!!window.TripettoClassic}`);

        TripettoStudio.form({
            runner: TripettoClassic,
            token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoib0kzNHo5eUVjVGZ0S0tjRXowNUJLdTlqSTdPMENHeHdmbHJCeWdtTThBND0iLCJkZWZpbml0aW9uIjoiUkZsK3BFUWJjRXJ6M3lOYVg2cyt4S0FGV3BYb1lPMVYyOGk5QVAvSmVFQT0iLCJ0eXBlIjoiY29sbGVjdCJ9.RhvMpqaD2Lr3JMrrtAnRITCMuDsU9AR9Ik1MIjSR-Ig",
            element: "tripetto-1wg3b90",
            // Add explicit onLoad and onError callbacks
            onLoad: function() {
                debugLog('Form successfully loaded');
            },
            onError: function(error) {
                debugLog(`Form initialization error: ${JSON.stringify(error)}`);
            }
        });

        const observer = new MutationObserver((mutations) => {
            mutations.forEach((mutation) => {
                // Log details of added nodes
                if (mutation.type === 'childList') {
                    mutation.addedNodes.forEach((node) => {
                        // Check if an iframe is added
                        if (node.nodeName === 'IFRAME') {
                            console.log('Iframe detected:', {
                                src: node.src,
                                id: node.id,
                                className: node.className
                            });

                            // Try to access iframe content
                            try {
                                const iframeDoc = node.contentDocument || node.contentWindow.document;

                                // Create a new observer for the iframe content
                                const iframeObserver = new MutationObserver((iframeMutations) => {
                                    console.log('Iframe content mutations:', iframeMutations);

                                    // Search for H2 elements within the iframe
                                    const iframeH2Elements = iframeDoc.getElementsByTagName('h2');
                                    console.log('H2 elements in iframe:', {
                                        count: iframeH2Elements.length,
                                        elements: Array.from(iframeH2Elements).map((el, index) => ({
                                            index: index + 1,
                                            textContent: el.textContent.trim(),
                                            matches: el.textContent.trim().includes(TRIGGER_TEXT)
                                        }))
                                    });

                                    // Check for wrap text in iframe
                                    const wrapElements = Array.from(iframeH2Elements)
                                        .filter(el => el.textContent.trim().includes(TRIGGER_TEXT));

                                    if (wrapElements.length > 0) {
                                        console.log('Wrap found in iframe!');
                                        iframeObserver.disconnect();
                                        replaceFormWithThankYou();
                                    }
                                });

                                // Observe the iframe document
                                iframeObserver.observe(iframeDoc.body, {
                                    childList: true,
                                    subtree: true,
                                    characterData: true
                                });

                            } catch (error) {
                                console.error('Error accessing iframe content:', error);
                            }
                        }
                    });
                }
            });
        });

        observer.observe(document.body, {
            childList: true,
            subtree: true,
            characterData: false
        });
    });
</script>

<a href="../" class="back-button">Go Back</a>