#==============================================================================
# ▶ Custom Side-View Battle Engine
#    Version: 1.0
#    Author: CrypticTM
#    Inspired by: Yanfly Battle Engine
#    Engine: RPG Maker VX Ace
#==============================================================================
# FEATURES:
# - Custom actor/enemy side-view placement
# - Turn Order Bar (AGI-based)
# - Skill Casting Animations via <CastAnim:X> note tag
#==============================================================================

module CustomSideView
  ACTOR_BASE_X = 120
  ACTOR_SPACING = 90
  ACTOR_Y = 280

  ENEMY_BASE_X = 400
  ENEMY_SPACING = 90
  ENEMY_Y = 280
end

#==============================================================================
# ▶ Sprite_Battler (for Side-View Placement)
#==============================================================================

class Sprite_Battler < Sprite_Base
  alias :csv_initialize :initialize
  def initialize(viewport, battler = nil)
    csv_initialize(viewport, battler)
    update_position if battler
  end

  def update_position
    return unless @battler
    if @battler.actor?
      self.x = CustomSideView::ACTOR_BASE_X + @battler.index * CustomSideView::ACTOR_SPACING
      self.y = CustomSideView::ACTOR_Y
      self.mirror = false
    else
      self.x = CustomSideView::ENEMY_BASE_X + @battler.index * CustomSideView::ENEMY_SPACING
      self.y = CustomSideView::ENEMY_Y
      self.mirror = true
    end
  end

  alias :csv_update :update
  def update
    csv_update
    update_position if @battler
  end
end

#==============================================================================
# ▶ Window_TurnOrder (AGI-based Turn Bar)
#==============================================================================

class Window_TurnOrder < Window_Base
  ICON_SIZE = 24

  def initialize
    super(0, 0, Graphics.width, fitting_height(1))
    refresh
  end

  def refresh
    contents.clear
    order = $game_party.battle_members + $game_troop.members
    order = order.select { |b| b.exist? }.sort_by { |b| -b.agi }

    order[0, 6].each_with_index do |battler, i|
      name = battler.name
      color = battler.actor? ? normal_color : crisis_color
      change_color(color)
      draw_text(i * 100 + 10, 0, 100, line_height, name, 1)
    end
  end
end

#==============================================================================
# ▶ Scene_Battle (Turn Bar + Casting Hook)
#==============================================================================

class Scene_Battle < Scene_BattleBase
  alias :csv_start :start
  def start
    csv_start
    create_turn_order_window
  end

  def create_turn_order_window
    @turn_order_window = Window_TurnOrder.new
    @turn_order_window.z = 200
  end

  alias :csv_use_item :use_item
  def use_item
    battler = @subject
    item = battler.current_action.item

    if item.is_a?(RPG::Skill)
      perform_casting_animation(battler, item)
    end

    csv_use_item
  end

  def perform_casting_animation(battler, skill)
    animation_id = skill.note[/<CastAnim:(\d+)>/, 1]
    return unless animation_id
    battler.perform_cast_pose
    targets = [battler]
    show_animation(targets, animation_id.to_i)
    wait(20)
  end
end

#==============================================================================
# ▶ Game_Battler (Cast Pose Action)
#==============================================================================

class Game_Battler < Game_BattlerBase
  def perform_cast_pose
    if actor?
      $game_party.members.each_with_index do |a, i|
        next unless a == self
        SceneManager.scene.spriteset.battler_sprites.each do |sprite|
          next unless sprite.battler == self
          sprite.start_animation($data_animations[1]) # default casting anim
        end
      end
    else
      SceneManager.scene.spriteset.battler_sprites.each do |sprite|
        next unless sprite.battler == self
        sprite.start_animation($data_animations[1])
      end
    end
    Sound.play_se({ name: "Wind7", volume: 80, pitch: 100 })
  end
end
