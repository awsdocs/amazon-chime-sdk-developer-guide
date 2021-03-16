# *videoTileDidUpdate* callback and binding video tiles<a name="video-tile-did-update"></a>

 When users turn on video, you receive the *videoTileDidUpdate* callback with a *tileState* object\. The *tileState* object has all information about the user's video stream, including: tileId, if the tile is local, if the tile is a content tile, if it's active or paused, size information, if it is bound to a video element, and so on\. The application has to manage the tiles as they get added, and bind them to HTML Video Elements using the *bindVideoElement* API of the AudioVideoFacade\. 

```
videoTileDidUpdate(tileState: VideoTileState): void {
    this.log(`video tile updated: ${JSON.stringify(tileState, null, '  ')}`);
    if (!tileState.boundAttendeeId) {
      return;
    }
    const selfAttendeeId = this.meetingSession.configuration.credentials.attendeeId;
    const modality = new DefaultModality(tileState.boundAttendeeId);
    if (modality.base() === selfAttendeeId && modality.hasModality(DefaultModality.MODALITY_CONTENT)) {
      // don't bind one's own content
      return;
    }
    const tileIndex = tileState.localTile
      ? 16
      : this.tileOrganizer.acquireTileIndex(tileState.tileId);
    const tileElement = document.getElementById(`tile-${tileIndex}`) as HTMLDivElement;
    const videoElement = document.getElementById(`video-${tileIndex}`) as HTMLVideoElement;
    const nameplateElement = document.getElementById(`nameplate-${tileIndex}`) as HTMLDivElement;

    const pauseButtonElement = document.getElementById(`video-pause-${tileIndex}`) as HTMLButtonElement;
    const resumeButtonElement = document.getElementById(`video-resume-${tileIndex}`) as HTMLButtonElement;

    pauseButtonElement.addEventListener('click', () => {
      if (!tileState.paused) {
        this.audioVideo.pauseVideoTile(tileState.tileId);
      }
    });

    resumeButtonElement.addEventListener('click', () => {
      if (tileState.paused) {
        this.audioVideo.unpauseVideoTile(tileState.tileId);
      }
    });

    this.log(`binding video tile ${tileState.tileId} to ${videoElement.id}`);
    this.audioVideo.bindVideoElement(tileState.tileId, videoElement);
    this.tileIndexToTileId[tileIndex] = tileState.tileId;
    this.tileIdToTileIndex[tileState.tileId] = tileIndex;
    // TODO: enforce roster names
    new TimeoutScheduler(2000).start(() => {
      const rosterName = this.roster[tileState.boundAttendeeId]
        ? this.roster[tileState.boundAttendeeId].name
        : '';
      if (nameplateElement.innerHTML !== rosterName) {
        nameplateElement.innerHTML = rosterName;
      }
    });
    tileElement.style.display = 'block';
    this.layoutVideoTiles();
  }
```