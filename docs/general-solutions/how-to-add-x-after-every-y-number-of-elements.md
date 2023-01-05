---
sidebar_label: 'How to add X after N number of elements'
sidebar_position: 1
---

# How to add X after N number of elements


## Gutenberg Method



## Old Editor Method

### Solution 1
```
function insert_ad_block( $text ) {

    if ( is_single() ) :

        $ads_text = '<div class="center">' .$block TODO . '</div>';
        $split_by = "\n";
        $insert_after = 3; //number of paragraphs

        // make array of paragraphs
        $paragraphs = explode( $split_by, $text);

            if ( count( $paragraphs ) > $insert_after ) {

                        $new_text = '';     // new text
                        $i = 1;             // current ad index

                        // loop through array and build string for output
                        foreach( $paragraphs as $paragraph ) {
                            // add paragraph, preceeded by an ad after every $insert_after
                            $new_text .= ( $i % $insert_after == 0 ? $ads_text : '' ) . $paragraph;
                            // increase index
                            $i++;
                        }

                        return $new_text;
            }


            // otherwise just add the ad to the end of the text
            return $text . $ads_text;

    endif;

    return $text;

}
add_filter('the_content', 'insert_ad_block');

add_filter('the_content', 'wpse_ad_content');

function wpse_ad_content($content)
{
    if (!is_single()) return $content;
    $paragraphAfter = 2; //Enter number of paragraphs to display ad after.
    $content = explode("</p>", $content);
    $new_content = '';
    for ($i = 0; $i < count($content); $i++) {
        if ($i == $paragraphAfter) {
            $new_content.= '<div style="width: 300px; height: 250px; padding: 6px 6px 6px 0; float: left; margin-left: 0; margin-right: 18px;">';
            $new_content.= '//Enter your ad code here....';
            $new_content.= '</div>';
        }

        $new_content.= $content[$i] . "</p>";
    }

    return $new_content;
}
```
Source https://wordpress.stackexchange.com/questions/252983/add-an-advert-every-nth-paragraph

https://wordpress.stackexchange.com/questions/58605/insert-ad-code-in-the-middle-of-a-post/58611#58611

If you're fine with the post being modded in archive pages, then you can skip is_single.

Another person asked 'How can I add a list of posts in the middle of the post. It's the same procedure, except that you add a query to pull your desired posts. This asker got it almost all the way there. But, they're missing something important. They're missing the split function on the paragraph tags.
https://wordpress.stackexchange.com/questions/395160/display-post-lists-in-2nd-paragraph/395237#395237

This one does it an intersting way

```
function addParagraphs($content) {
    // you can add as many as you want:
    $additions = array(
        '<p>After 1st paragraph</p>',
        '<p>After 2nd paragraph</p>'
    );

    $content = get_the_content();

    $output = ''; // define variable to avoid PHP warnings

    $parts = explode("</p>", $content);

    $count = count($parts); // call count() only once, it's faster

    for($i=0; $i<$count; $i++) {
        $output .= $parts[$i] . '</p>' . $additions[$i]; // non-existent additions does not concatenate
    }
    return $output;

}
add_filter('the_content','addParagraphs');
```

Another approach is to do the change in the template where the content is being output
$paragraphAfter[1] = '<div>AFTER FIRST</div>'; //display after the first paragraph
$paragraphAfter[3] = '<div>AFTER THIRD</div>'; //display after the third paragraph
$paragraphAfter[5] = '<div>AFTER FIFtH</div>'; //display after the fifth paragraph

$content = apply_filters( 'the_content', get_the_content() );
$content = explode("</p>", $content);
$count = count($content);
for ($i = 0; $i < $count; $i++ ) {
    if ( array_key_exists($i, $paragraphAfter) ) {
        echo $paragraphAfter[$i];
    }
    echo $content[$i] . "</p>";
}

```

it can split on </p> but only if you make sure to add it back.

#### How it works:

First we get access to the page content on every load with the 'the_content' filter. Next, it checks that the page displays a single post item and bails out if it's not.

If it's the right place we split the content into paragraphs. In the old editor paragraphs are split by new lines. Thats why you can just run the split function on the string representing newlines\n to produce an array that has 1 paragraph per item. Then the rest is as simple as counting up 3 and inserting your desired element.

Another asker asks Adding ads after a certain number of paragraphs within Genesis themework
And, it's the same method. Genesis puts tons of convenient hooks outside the editor. But it doesn't add any custom code inside the middle of editor. 


## Solution 2

```
/** 
 * Class AdsINParagraphs
 * 
 * Class to inject ads into single post's content between paragraphs 
 * as required
 */
class GoogleAdsINParagraphs
{
     /** 
     * @var string $ad
     * @access protected
     * @since 1.1.0
     */
    protected $ad;

     /** 
     * @var int|string $min
     * @access protected
     * @since 1.1.0
     */
    protected $min;

     /** 
     * @var array $inject
     * @access protected
     * @since 1.1.0
     */
    protected $inject;

     /** 
     * @var $post = NULL
     * @access protected
     * @since 1.1.0
     */
    protected $post = NULL;

     /** 
     * @var $paragraphs = NULL
     * @access protected
     * @since 1.1.0
     */
    protected $paragraphs = NULL;

     /** 
     * @var $paraCount = 0
     * @access protected
     * @since 1.1.0
     */
    protected $paraCount = 0;

     /** 
     * @var $contentReplacement = NULL
     * @access protected
     * @since 1.1.0
     */
    protected $contentReplacement = NULL;

    /**
     * Public method __construct()
     *
     * Constructor for the GoogleAdsINParagraphs class
     *
     * @param string     $ad     The add that should be injected
     * @param string|int $min    The minimum amount of paragraphs that must exist
     * @param array      $inject An array of paragraph numbers where the add should be injected
     *                           If you need an ad before the content, simply pass 0 as first value
     * @access public
     * @since 1.1.0
     */
    public function __construct( $ad = NULL, $min = 4, $inject = [] )
    {
        $this->ad     = $ad;
        $this->min    = $min;
        $this->inject = $inject;
    }

    /**
     * Public method init()
     *
     * Initialize the 'the_content' filter callback
     *
     * @access public
     * @since 1.1.0
     */
    public function init()
    {
        add_filter( 'the_content', [$this, 'postContent'] );
    }

    /**
     * Protected method initHelperMethods()
     *
     * Initialize all the helper functions for the 'the_content' filter callback
     *
     * @access protected
     * @since 1.1.0
     */
    protected function initHelperMethods()
    {
        $this->post();
        $this->paragraphs();
        $this->paraCount();
        $this->contentReplacement();
    }


    /**
     * Protected method post()
     *
     * Gets the current post object and sanitize the post object. Sanitation 
     * is done as precaution should the query object being altered by some other
     * program/function/malicious code
     *
     * @access protected
     * @since 1.1.0
     */
    protected function post()
    {
        $post_raw   = get_queried_object();
        // Sanitize the post object in case that something externally changed the queried object
        $post       = sanitize_post( $post_raw );
        $this->post = $post;
    }

    /**
     * Protected method paragraphs()
     *
     * Gets the current post's post content, sanitizes it and then applies the wpautop filter
     * function to it to apply p tags to the raw content. After the filter function is applied,
     * the content is exploded into an array of paragraphs
     *
     * @access protected
     * @since 1.1.0
     */
    protected function paragraphs()
    {
        // Just in case, as extra precaution, run the raw post comment through wp_kses_post 
        $noEvilContent    = wp_kses_post( $this->post->post_content );
        $peedContent      = wpautop( $noEvilContent );
        $paragraphs       = explode( '<p>', $peedContent );

        $this->paragraphs = $paragraphs;
    }

    /**
     * Protected method paraCount()
     *
     * Counts the amount of paragraphs created by the paragraphs() method
     *
     * @access protected
     * @since 1.1.0
     */
    protected function paraCount()
    {
        $paraCount       = count( $this->paragraphs );

        $this->paraCount = (int) $paraCount;
    }

    /**
     * Protected method contentReplacement()
     *
     * This is where everything happens. The following checks are run
     * before ads are inserted into the content
     * - Check if there is at least the set minimum amount of paragraphs. 
     *   Checks are done against $this->min
     * - Check if there at least enough paragraphs to fullfil the lowest
     *   passed number in $this->inject
     * - Check to see if $this->inject is a valid array with values
     *
     * If all checks pass, ads is inserted into the content. If any value
     * in $this->inject is more than the amount of paragraphs, the ad is ignored
     * and not inserted
     *
     * @access protected
     * @since 1.1.0
     */
    protected function contentReplacement()
    {
        // Lets first run all our checks 

        $this->contentReplacement = $contentReplacement = NULL;

        // Make sure that we have at least more paragraphs than our minimum, bail if not
        if ( $this->paraCount < $this->min ) 
            return $this->contentReplacement;

        // Make sure that $this->inject is an array and not empty
        if (    !is_array( $this->inject ) 
             || !$this->inject
        )
            return $this->contentReplacement;

        /**
         * We will do the following checks and validation on $this->inject
         * - validate the array of ID's and remove negative numbers
         * - sort $this->inject array numerically ASC according to passed ID's 
         * - remove possible dublicates
         */
        $this->inject = filter_var( 
            $this->inject, 
            FILTER_VALIDATE_INT, 
            [
                'flags' => FILTER_REQUIRE_ARRAY,
                'options' => [
                    'min_range' => 0
                ]
            ]
        ); 
        asort( $this->inject );
        $this->inject = array_unique( $this->inject );

        // Make sure that we actaully have enough paragraphs to inject an add into 

        if ( $this->paraCount < $this->inject[0] )
            return $this->contentReplacement;

        // Now we can start to inject our ads into our content. Set $output as empty string
        $output = '';
        foreach ( $this->paragraphs as $key=>$paragraph ) {
            // Add ad in front of content if $this->inject[0] === 0
            if (    0 === $key 
                 && 0 === $this->inject[0]
            ) {
                $output .= '<p>' . $this->ad . '</p>' . $paragraph;
            } elseif ( !in_array( $key, $this->inject ) ) {
                $output .= $paragraph; 
            } else {
                $output .= $paragraph . '<p>' . $this->ad . '</p>';
            }
        } // endforeach

        $this->contentReplacement = $output;
    }

   /**
     * Public method postContent()
     *
     * This is the callback method for the 'the_content' filter. The content passed by reference
     * inside the filter is replaced by any valid value from $this->contentReplacement. This is done
     * purely for convenience
     *
     * @access public
     * @since 1.1.0
     * @return $content
     */
    public function postContent( $content )
    {
        // Lets first run our checks, make sure this is a single page and the main query
        if ( !is_single() )
            return $content;

        if ( !in_the_loop() )
            return $content;

        // Initialize our other helper methods. They should only run inside our condition
        $this->initHelperMethods();

        // Make sure $this->contentReplacement is not NULL, if so, just return $content
        if ( NULL === $this->contentReplacement )
            return $content;

        // We can now replace $content with $this->contentReplacement
        $content = $this->contentReplacement;

        return $content;
    }
} // end of class GoogleAdsINParagraphs

$ad      = 'PLACE YOUR AD HERE';
$add_ads = new GoogleAdsINParagraphs( 
    $ad,     // Your ad which you want to add
    4,       // There should be at least 4 paragraphs
    [4, 20]  // Array of paragraph numbers after which you want to inject an add
);
$add_ads->init();
```
Source: https://wordpress.stackexchange.com/questions/215732/inserting-ads-within-content
### How it works.

Every post