# Life Rendering: Product Specification

## The Core Product Insight

**The Insight**: Your quantified self data (Whoop, GitHub, Screen Time, Calendar) isn't just metrics - it's the raw material for a new form of creative expression. Your actual lived experience can be transformed into shareable temporal art that reveals patterns you couldn't see before.

**The Paradigm Shift**:
- **From**: "I track my life" → **To**: "My life creates art"
- **From**: "I make content" → **To**: "My existence generates beauty"
- **From**: Explicit creation → **To**: Implicit revelation

**Why Now**:
1. Quantified self is mainstream (everyone has health/productivity data)
2. People are exhausted by performative social media
3. AI can now generate beautiful visuals at scale
4. Mental health awareness makes "sharing real days" culturally acceptable
5. Abstract content is shareable (TikTok trained us to watch anything for 20 seconds)

**The Unique Value**: This isn't another AI video tool. It's the first platform that transforms your actual lived experience into shareable art without any creative input from you. You don't create - you live, and your life becomes the creation.

---

## The Video Artifact: "My Tuesday"

### Format Specifications
- **Duration**: 20 seconds
- **Resolution**: 1080x1920 (9:16 vertical for mobile/TikTok)
- **Frame Rate**: 60fps for smooth transitions
- **Audio**: Ambient generative soundtrack based on biometric rhythms
- **File Size**: ~15MB (optimized for instant sharing)

### Visual Language

The video is **NOT**:
- Narrative or story-based
- Character-driven
- Representational
- Entertainment

The video **IS**:
- Abstract temporal art
- Data-driven visualization
- Emotional fingerprinting
- Pattern revelation

### Scene-by-Scene Breakdown

**[0:00-0:03] Morning Recovery State**
- **Data Source**: Whoop recovery score (0-100%), HRV (20-200ms)
- **Visual**: Breathing organic circles, cellular patterns
- **Color**: Green (high recovery) to Red (low recovery) spectrum
- **Motion**: Slow expansion/contraction matching HRV rhythm
- **Texture**: Soft, organic, living
- **Overlaid Data**: "Tuesday 6:47am | Recovery: 78%"

**[0:03-0:06] Physical Activity**
- **Data Source**: Workout type, heart rate zones, strain (0-21)
- **Visual**: Particle explosions radiating from center
- **Color**: Warm oranges/reds intensifying with heart rate
- **Motion**: Rhythmic pulses matching step cadence or rep timing
- **Texture**: Dynamic, energetic, explosive
- **Overlaid Data**: "5.2mi | Zone 3 | Strain: 14.2"

**[0:06-0:10] Deep Work/Flow State**
- **Data Source**: GitHub commits, lines of code, app focus time
- **Visual**: Geometric patterns constructing themselves
- **Color**: Cool cyans and purples for focus
- **Motion**: Precise, architectural, building progressively
- **Texture**: Crystalline, sharp, organized
- **Overlaid Data**: "247 lines | 12 commits | 3hr focus"

**[0:10-0:13] Context Switching/Meetings**
- **Data Source**: Calendar density, app switching frequency
- **Visual**: Fragmenting grids, overlapping rectangles
- **Color**: Desaturating from full color to grays
- **Motion**: Choppy, interrupted, multiple directions
- **Texture**: Fragmented, noisy, chaotic
- **Overlaid Data**: "3 meetings | 47 context switches"

**[0:13-0:16] Problem/Challenge State**
- **Data Source**: Error logs, failed builds, stress indicators
- **Visual**: Glitch effects, digital corruption
- **Color**: Warning reds, error oranges
- **Motion**: Stuttering, repeating, then breakthrough
- **Texture**: Distorted, pixelated, then suddenly clear
- **Overlaid Data**: "Deploy failed → Fixed: 16:34"

**[0:16-0:19] Recovery/Wind-down**
- **Data Source**: Evening HRV, screen time apps, media consumption
- **Visual**: Soft watercolor bleeds, gentle flows
- **Color**: Warm pastels, sunset tones
- **Motion**: Slow dissolves, organic drifting
- **Texture**: Blurred, soft, comfortable
- **Overlaid Data**: "Netflix: 2hr | HRV rising"

**[0:19-0:20] Day Complete**
- **Data Source**: All day metrics compressed
- **Visual**: All colors/patterns spiral into single point
- **Color**: White light containing all day's colors
- **Motion**: Compression to center, single pulse
- **Texture**: Pure, simple, complete
- **Overlaid Data**: "Tuesday: Complete | sharelatent.com"

### Audio Design

**Not music, but "biometric ambience"**:
- Base frequency determined by average HRV
- Rhythm follows heart rate patterns
- Intensity correlates with strain
- Harmonics shift with recovery score
- Transitions match visual scene changes

---

## Engineering Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     Data Ingestion Layer                      │
├───────┬───────┬──────────┬──────────┬───────────┬───────────┤
│ Whoop │GitHub │ Calendar │ Screen   │ Spotify   │ Location  │
│  API  │  API  │   API    │  Time    │   API     │   API     │
└───────┴───────┴──────────┴──────────┴───────────┴───────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                   Pattern Extraction Engine                   │
│  • Identify key moments (PRs, crashes, flow states)          │
│  • Calculate emotional arc of day                            │
│  • Extract rhythm and pacing                                 │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    Render Orchestrator                        │
│  • Segment day into scenes                                   │
│  • Assign appropriate renderers                              │
│  • Calculate transitions                                     │
└─────────────────────────────────────────────────────────────┘
                              ↓
        ┌──────────────┬──────────────┬──────────────┐
        │   Shader     │      AI      │    Audio     │
        │   Engine     │  Enhancement │  Synthesis   │
        └──────────────┴──────────────┴──────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                     Compositing Pipeline                      │
│  • Blend shader output with AI frames                        │
│  • Apply temporal coherence                                  │
│  • Sync audio to visuals                                     │
│  • Export final video                                        │
└─────────────────────────────────────────────────────────────┘
```

### Component Details

#### 1. Data Ingestion Layer
```python
class DataCollector:
    def __init__(self, user_id):
        self.collectors = {
            'whoop': WhoopCollector(user_id),
            'github': GitHubCollector(user_id),
            'calendar': CalendarCollector(user_id),
            'screentime': ScreenTimeCollector(user_id),
            'spotify': SpotifyCollector(user_id)
        }
    
    async def collect_day_data(self, date):
        """Collect all data for a specific day"""
        tasks = []
        for name, collector in self.collectors.items():
            tasks.append(collector.get_day_data(date))
        
        results = await asyncio.gather(*tasks)
        return self.normalize_data(results)
    
    def normalize_data(self, raw_data):
        """Convert to standard format"""
        return {
            'biometric': {
                'recovery': raw_data.whoop.recovery_score,
                'hrv': raw_data.whoop.hrv_samples,
                'heart_rate': raw_data.whoop.heart_rate_samples,
                'strain': raw_data.whoop.strain_score
            },
            'productivity': {
                'commits': raw_data.github.commits,
                'lines_changed': raw_data.github.lines,
                'focus_time': raw_data.screentime.focus_duration,
                'meeting_time': raw_data.calendar.meeting_duration
            },
            'emotional': {
                'stress_moments': self.detect_stress(raw_data),
                'flow_periods': self.detect_flow(raw_data),
                'energy_curve': self.calculate_energy(raw_data)
            }
        }
```

#### 2. Shader Engine (Real-time Generation)
```javascript
// WebGL Shader for Workout Visualization
class WorkoutShader {
  constructor(canvas) {
    this.gl = canvas.getContext('webgl2');
    this.program = this.createShaderProgram();
    this.particles = new Float32Array(10000 * 3); // 10k particles
    this.initializeParticles();
  }

  vertexShaderSource = `
    attribute vec3 position;
    uniform float time;
    uniform float heartRate;
    uniform float strain;
    uniform mat4 mvpMatrix;
    
    varying vec3 vColor;
    
    void main() {
      // Pulse based on heart rate
      float pulse = sin(time * heartRate / 60.0 * 3.14159);
      vec3 pos = position;
      
      // Explode outward based on strain
      pos *= 1.0 + pulse * strain * 0.05;
      
      // Add movement
      pos.x += sin(time + position.y) * strain * 0.1;
      pos.y += cos(time + position.x) * strain * 0.1;
      
      // Color based on heart rate zones
      if (heartRate < 120.0) {
        vColor = vec3(0.2, 0.8, 0.2); // Green - Zone 1
      } else if (heartRate < 140.0) {
        vColor = vec3(0.8, 0.8, 0.2); // Yellow - Zone 2  
      } else if (heartRate < 160.0) {
        vColor = vec3(0.9, 0.5, 0.1); // Orange - Zone 3
      } else {
        vColor = vec3(0.9, 0.1, 0.1); // Red - Zone 4+
      }
      
      gl_Position = mvpMatrix * vec4(pos, 1.0);
      gl_PointSize = 2.0 + strain * 0.5;
    }
  `;

  fragmentShaderSource = `
    precision highp float;
    varying vec3 vColor;
    
    void main() {
      // Soft circular points
      vec2 coord = gl_PointCoord - vec2(0.5);
      if (length(coord) > 0.5) discard;
      
      float alpha = 1.0 - length(coord) * 2.0;
      gl_FragColor = vec4(vColor, alpha);
    }
  `;

  render(biometricData, duration) {
    const frames = [];
    const fps = 60;
    const totalFrames = duration * fps;
    
    for (let frame = 0; frame < totalFrames; frame++) {
      const time = frame / fps;
      const heartRate = this.interpolateHeartRate(biometricData, time);
      const strain = this.interpolateStrain(biometricData, time);
      
      this.gl.uniform1f(this.timeUniform, time);
      this.gl.uniform1f(this.heartRateUniform, heartRate);
      this.gl.uniform1f(this.strainUniform, strain);
      
      this.gl.clear(this.gl.COLOR_BUFFER_BIT);
      this.gl.drawArrays(this.gl.POINTS, 0, this.particles.length / 3);
      
      frames.push(this.captureFrame());
    }
    
    return frames;
  }
}
```

#### 3. AI Enhancement Layer
```python
class AIEnhancer:
    def __init__(self):
        self.flux = FluxModel()
        self.style_refs = self.load_style_references()
    
    def identify_key_moments(self, day_data):
        """Find moments worth AI enhancement"""
        moments = []
        
        # Flow state achievement
        if day_data.max_focus_duration > 180:  # 3+ hour focus
            moments.append({
                'time': day_data.flow_start_time,
                'type': 'flow_state',
                'prompt': self.generate_flow_prompt(day_data)
            })
        
        # Physical breakthrough
        if day_data.workout_pr:
            moments.append({
                'time': day_data.pr_time,
                'type': 'breakthrough',
                'prompt': self.generate_breakthrough_prompt(day_data)
            })
        
        # Recovery milestone
        if day_data.recovery > 90:
            moments.append({
                'time': '06:00',
                'type': 'peak_recovery',
                'prompt': self.generate_recovery_prompt(day_data)
            })
        
        return moments
    
    def generate_keyframes(self, moments, user_aesthetic):
        """Generate beautiful AI frames for key moments"""
        keyframes = []
        
        for moment in moments:
            prompt = f"""
            {moment.prompt}
            Style: {user_aesthetic.style_preference}
            Color palette: {user_aesthetic.color_preference}
            Complexity: {user_aesthetic.complexity_level}
            Reference: Abstract data art, Refik Anadol, James Turrell
            """
            
            image = self.flux.generate(
                prompt=prompt,
                guidance_scale=7.5,
                style_reference=user_aesthetic.reference_image
            )
            
            keyframes.append({
                'time': moment.time,
                'image': image,
                'type': moment.type
            })
        
        return keyframes
```

#### 4. Temporal Compositor
```python
class TemporalCompositor:
    def __init__(self):
        self.ffmpeg = FFMpegWrapper()
        self.flow_interpolator = OpticalFlowInterpolator()
    
    def compose_day_video(self, shader_segments, ai_keyframes, audio_track):
        """Combine all elements into final video"""
        
        # Base timeline from shader renders
        timeline = Timeline()
        for segment in shader_segments:
            timeline.add_clip(segment.frames, segment.start_time, segment.duration)
        
        # Blend in AI keyframes at specific moments
        for keyframe in ai_keyframes:
            # Create 2-second transition around keyframe
            transition_start = keyframe.time - 1.0
            transition_end = keyframe.time + 1.0
            
            # Use optical flow to morph from shader to AI frame
            morph = self.flow_interpolator.create_morph(
                from_frame=timeline.get_frame_at(transition_start),
                to_frame=keyframe.image,
                duration=2.0,
                style='smooth_morph'
            )
            
            timeline.overlay_clip(morph, transition_start)
        
        # Add data overlays
        timeline = self.add_data_overlays(timeline, shader_segments)
        
        # Sync audio
        timeline.set_audio_track(audio_track)
        
        # Export final video
        return self.ffmpeg.export(
            timeline,
            format='mp4',
            codec='h264',
            bitrate='8M',
            resolution=(1080, 1920),
            fps=60
        )
```

#### 5. Audio Synthesis
```python
class BiometricAudioGenerator:
    def __init__(self):
        self.synthesizer = ToneSynthesizer()
    
    def generate_day_soundtrack(self, biometric_data, duration=20):
        """Create ambient audio from biometric rhythms"""
        
        # Base drone frequency from average HRV
        base_freq = self.hrv_to_frequency(biometric_data.avg_hrv)
        
        # Create evolving drone
        drone = self.synthesizer.create_drone(
            frequency=base_freq,
            duration=duration,
            envelope=self.create_day_envelope(biometric_data)
        )
        
        # Add rhythmic elements from heart rate
        rhythm = self.synthesizer.create_rhythm(
            pattern=self.heartrate_to_rhythm(biometric_data.heart_samples),
            tempo=biometric_data.avg_heart_rate
        )
        
        # Add texture from activity changes
        texture = self.synthesizer.create_texture(
            density=biometric_data.activity_changes,
            brightness=biometric_data.recovery_score / 100
        )
        
        # Mix layers
        return self.synthesizer.mix([
            (drone, 0.5),
            (rhythm, 0.3),
            (texture, 0.2)
        ])
    
    def hrv_to_frequency(self, hrv):
        """Map HRV to musical frequency"""
        # Higher HRV = lower, calmer frequencies
        # Lower HRV = higher, tense frequencies
        return 440 * (2 ** ((60 - hrv) / 60))
```

### Production Pipeline

#### Real-time Preview (Instant)
```python
async def generate_preview(user_id, current_data):
    """5-second preview updated hourly"""
    shader = get_shader_for_activity(current_data.current_activity)
    frames = shader.render(current_data, duration=5)
    return frames_to_video(frames)
```

#### Daily Render (End of Day)
```python
async def generate_daily_render(user_id, date):
    """Full 20-second day visualization"""
    
    # Collect all day data
    day_data = await DataCollector(user_id).collect_day_data(date)
    
    # Extract patterns
    patterns = PatternExtractor().extract(day_data)
    
    # Generate shader base
    shader_segments = ShaderEngine().render_day(patterns)
    
    # Enhance with AI
    key_moments = AIEnhancer().identify_key_moments(patterns)
    ai_frames = AIEnhancer().generate_keyframes(key_moments)
    
    # Generate audio
    audio = BiometricAudioGenerator().generate_day_soundtrack(day_data)
    
    # Composite final video
    video = TemporalCompositor().compose_day_video(
        shader_segments,
        ai_frames,
        audio
    )
    
    # Upload to sharelatent.com
    url = await upload_to_cdn(video)
    
    return url
```

### Cost Analysis

#### Per Daily Render
- **Shader Generation**: $0.001 (GPU compute)
- **AI Keyframes** (3-5 frames): $0.05 (Flux API)
- **Audio Synthesis**: $0.001 (CPU compute)
- **Video Encoding**: $0.002 (FFmpeg)
- **Storage/CDN**: $0.01
- **Total**: ~$0.06 per daily video

#### At Scale (10,000 daily active users)
- **Daily Cost**: $600
- **Monthly Cost**: $18,000
- **Revenue** (at $10/month): $100,000
- **Gross Margin**: 82%

### Infrastructure Requirements

#### Core Services
- **GPU Cluster**: For shader rendering (4x A100 or consumer 4090s)
- **AI API**: Flux or Stable Diffusion endpoint
- **Queue System**: Redis + Celery for job processing
- **Storage**: S3-compatible for video storage
- **CDN**: CloudFlare for global distribution

#### Scaling Strategy
1. **Shader rendering**: Horizontally scalable, add GPUs as needed
2. **AI enhancement**: Use API with burst capacity
3. **Audio**: CPU-based, easily scalable
4. **Compositing**: Parallel processing per user

### Deployment Architecture
```yaml
# Docker Compose for Local Development
version: '3.8'

services:
  data-collector:
    build: ./services/data-collector
    environment:
      - REDIS_URL=redis://redis:6379
    depends_on:
      - redis
      - postgres

  shader-engine:
    build: ./services/shader-engine
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu]
    volumes:
      - ./shaders:/app/shaders

  ai-enhancer:
    build: ./services/ai-enhancer
    environment:
      - FLUX_API_KEY=${FLUX_API_KEY}

  compositor:
    build: ./services/compositor
    volumes:
      - ./temp:/tmp/renders

  api:
    build: ./services/api
    ports:
      - "8000:8000"
    depends_on:
      - redis
      - postgres

  redis:
    image: redis:alpine

  postgres:
    image: postgres:14
    environment:
      - POSTGRES_DB=latent
      - POSTGRES_PASSWORD=${DB_PASSWORD}
```

### The Technical Moat

1. **Shader Library**: Custom-built for biometric visualization
2. **Pattern Recognition**: Proprietary algorithms for life patterns
3. **Aesthetic Fingerprinting**: Personal style learning
4. **Temporal Coherence**: Smooth blending between life moments
5. **Data Integration**: Deep integration with multiple APIs

---

## Success Metrics

### User Engagement
- **Daily Active Renderers**: Users who generate daily videos
- **Share Rate**: % of renders shared to sharelatent.com
- **Retention**: Users still active after 30 days
- **Gallery Growth**: Average renders per user over time

### Quality Metrics
- **Render Time**: < 30 seconds for daily video
- **Visual Quality**: User rating of aesthetic appeal
- **Pattern Accuracy**: Correlation between data and visuals
- **Share Worthiness**: % of videos users want to share

### Business Metrics
- **Cost per Render**: Target < $0.10
- **Conversion Rate**: Free preview → paid daily renders
- **LTV**: > $100 per user
- **Viral Coefficient**: > 1.5 (each user brings 1.5 more)

---

## The Vision

In one year, millions of people wake up to see their yesterday transformed into 20 seconds of beautiful, abstract art. They don't just track their lives - they see their lives as aesthetic experiences. Every workout becomes a visual explosion. Every coding session becomes architectural art. Every day becomes a shareable piece of their personal gallery.

This isn't content creation. It's existence visualization. It's life rendering. It's the most honest form of self-expression possible - because you're not expressing anything. You're just living, and your life is expressing itself.

**sharelatent.com** becomes the place where humanity shares not their curated highlights, but their actual days, rendered beautiful.