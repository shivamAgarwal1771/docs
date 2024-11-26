 {selectedMedia === 'video' && (
        <div className="media-selection-content">
        <VideotoJson/>
        </div>
      )}

      {selectedMedia === 'text' && (
        <div className="media-selection-content">
    <TexttoSpeech/>
        </div>
      )}
