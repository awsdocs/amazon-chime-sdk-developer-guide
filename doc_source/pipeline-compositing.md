# Enabling compositing for media pipelines<a name="pipeline-compositing"></a>

The Amazon Chime SDK media pipelines support compositing audio, video, and content share streams, but you must use Grid view\. The view uses a default video resolution of 1280x720, but you can also use 1920x1080\.

Grid view provides different screen layouts, depending on whether you enable content sharing\.
+ **No content sharing** – Each video stream occupies a tile and scales automatically, based on the number of tiles\. Tiles are automatically arranged in rows and columns, again based on number of tiles\. The maximum number of video tiles is 25\.  
![\[A 4-column, 4-row grid showing the outlines of people.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/grid-no-content-share.png)
+ **Content sharing enabled** – When you enable content share, you have the following layout options\.
  + `PresenterOnly` displays the content share and the presenter’s video\. You can use `topLeft`, `topRight`, `bottomLeft`, and `bottomRight` as locations, with `topRight` the default\.  
![\[Image showing a large video tile in the center of a window and a small tile in the upper-right.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/grid-presenter-only.png)

    The following example shows how to implement the layout programmatically\.

    ```
    {
        "CompositedVideo": {
        "Layout": "GridView",
        "Resolution": "FHD",
         "GridViewConfiguration": {
             "ContentShareLayout": "PresenterOnly",
             "PresenterOnlyConfiguration": { 
                 "PresenterPosition": "TopRight"
                 }
             }
         }
    }
    ```
  + `Horizontal` displays the content share and the four most recent video streams below the share\. Back\-end logic sorts the video tiles by the times at which attendees enable their video\. Presenters appear on the left when they enable video\.  
![\[Image showing a large video tile in the center of a window and 4 smaller tiles in a line below.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/grid-horizontal.png)

    The following example shows how to implement the horizontal layout programmatically\.

    ```
    {
         "CompositedVideo": {
         "Layout": "GridView",
          "Resolution": "FHD",
          "GridViewConfiguration": {
              "ContentShareLayout": "Horizontal"
              }
          }
    }
    ```
  + `Vertical` displays the content share and the four most recent videos stacked on the right\. Back\-end logic sorts the video tiles by the times at which attendees enable their video\. Presenters appear on the first tile when they enable video\.  
![\[Image showing a large video tile in the center of a window and 4 smaller tiles stacked on the right.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/grid-vertical.png)

    The following example shows how to implement the vertical layout programmatically\.

    ```
    {
         "CompositedVideo": {
         "Layout": "GridView",
          "Resolution": "FHD",
          "GridViewConfiguration": {
              "ContentShareLayout": "Vertical"
              }
          }
    }
    ```