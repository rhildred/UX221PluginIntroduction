## Wordpress Content plugins

Plugins in Wordpress are done using the callback or delegate pattern. 

<a title="The original uploader was Guanaco at English Wikipedia. [Public domain], via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:QuiznosSub.jpg"><img alt="QuiznosSub" src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/28/QuiznosSub.jpg/512px-QuiznosSub.jpg"></a>

Think of going to Quiznos and ordering this lovely sandwich. You will get a number. They will call your number when the sub is done. Same thing in code! 

```
<?php
/*
Plugin Name: Days Until PiDay
*/
add_action('init', function(){
    add_shortcode('piday-plugin', function($atts = [], $content = null){
        //put the attributes into javascript in case they are needed client side
        $content .= "<script>var atts = " .
            json_encode($atts) .
            "</script>";
        $daysuntilpiday = ceil((mktime(0,0,0,3,14,date('Y')) - time()) / 86400);
        $content .= $daysuntilpiday;
        return $content;
    });
    
});

```

One "orders" the piday plugin by activating it and putting a shortcode, `[piday-plugin]`, in your content. 

![the shortcode](https://rhildred.github.io/UX320AngularPlugin/READMEImages/DaysTillPiDay.png "the shortcode")

But wait! When we view the results there are -9 days till piday. 

![-9 days](https://rhildred.github.io/UX320AngularPlugin/READMEImages/Negative9Days.png "-9 days")

We need to use the programming pattern called "selection" to deal with the case where pi-day is in the past.

```

<?php
/*
Plugin Name: Days Until PiDay
*/

add_action('init', function(){
    add_shortcode('piday-plugin', function($atts = [], $content = null){
        //put the attributes into javascript in case they are needed client side
        $content .= "<script>var atts = " .
            json_encode($atts) .
            "</script>";
        $daysuntilpiday = ceil((mktime(0,0,0,3,14,date('Y')) - time()) / 86400);
        if($daysuntilpiday < 0){
            $daysuntilpiday = ceil((mktime(0,0,0,3,14,date('Y') + 1) - time()) / 86400);
        }
        $content .= $daysuntilpiday;
        return $content;
    });

});


```

The programming patterns of selection and delegate are part of a set of basic patterns that we typically use for wordpress plugins. Another pattern that we used without knowing it is called sequence. In the sequence pattern, which governs pretty much imperative programming code is executed an expression at a time from left to right top to bottom. For instance:

```

<?php
/*
Plugin Name: Sequence Plugin
*/

add_action('init', function(){
    add_shortcode('sequence-plugin', function($atts = [], $content = null){
        //put the attributes into javascript in case they are needed client side
        $content .= "<script>var atts = " .
            json_encode($atts) .
            "</script>";
        $content .= "This is line 1<br />";
        $content .= "This is line 2<br />";
        $content .= "This is line 3<br />";
        return $content;
    });

});


```

This produces the output:

![left to right ... top to bottom](https://rhildred.github.io/UX320AngularPlugin/READMEImages/SequencePlugin.png "left to right ... top to bottom")

In class we also did a plugin using the repetition pattern.

There is another more advanced plugin in this week's work wp-content/plugins/carServiceForm. We ran out of time so didn't take this up. One interesting point is that it makes use of attributes. The following lump of code injects attributes into javascript so that they can be consumed in our plugin client side:

```

        //put the attributes into javascript in case they are needed client side
        $content .= "<script>var atts = " .
            json_encode($atts) .
            "</script>";


```
For instance, `[sequence-plugin Line1=test1 Line2=test2]` will result in the following javascript being inserted:

```
<script>var atts = {"line1":"test1","line2":"test2"}</script>

```

We build on that to do the car service form.

Content plugins are an important part of WordPress. They can be used to add options in e-commerce. They can even be used to fill out forms for e-government.

